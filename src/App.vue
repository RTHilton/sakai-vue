<template>
	<div :class="containerClass" @click="onWrapperClick">
		<AppTopBar @menu-toggle="onMenuToggle" />
		<div class="layout-sidebar" @click="onSidebarClick">
			<AppMenu :model="menu" @menuitem-click="onMenuItemClick" />
		</div>

		<div class="layout-main-container">
			<div class="layout-main">
				<router-view />
			</div>
			<AppFooter />
		</div>

		<AppConfig
			:layoutMode="layoutMode"
			:layoutColorMode="layoutColorMode"
			@layout-change="onLayoutChange"
			@layout-color-change="onLayoutColorChange"
		/>
		<transition name="layout-mask">
			<div class="layout-mask p-component-overlay" v-if="mobileMenuActive"></div>
		</transition>
	</div>
</template>

<script lang="ts">
import { defineComponent, ref, watch, computed, onBeforeUpdate, inject } from 'vue'
import { useToast } from 'primevue/usetoast'
import { usePrimeVue } from 'primevue/config'
import { useRoute } from 'vue-router'
import AppTopBar from './AppTopbar.vue'
import AppMenu from './AppMenu.vue'
import AppConfig from './AppConfig.vue'
import AppFooter from './AppFooter.vue'

export default defineComponent({
	name: 'App',
	components: {
		AppTopBar,
		AppMenu,
		AppConfig,
		AppFooter
	},
	setup() {
		const route = useRoute()
		const appState = inject("$appState") as { theme: string }
		const toast = useToast()
		const primevue = usePrimeVue()
		const layoutMode = ref('static')
		const layoutColorMode = ref('light')
		const staticMenuInactive = ref(false)
		const overlayMenuActive = ref(false)
		const mobileMenuActive = ref(false)
		let menuClick = false

		const menu = [
				{
					label: 'Home',
					items: [{
						label: 'Dashboard', icon: 'pi pi-fw pi-home', to: '/'
					}]
				},
				{
					label: 'UI Kit', icon: 'pi pi-fw pi-sitemap',
					items: [
						{ label: 'Form Layout', icon: 'pi pi-fw pi-id-card', to: '/formlayout' },
						{ label: 'Input', icon: 'pi pi-fw pi-check-square', to: '/input' },
						{ label: "Float Label", icon: "pi pi-fw pi-bookmark", to: "/floatlabel" },
						{ label: "Invalid State", icon: "pi pi-fw pi-exclamation-circle", to: "invalidstate" },
						{ label: 'Button', icon: 'pi pi-fw pi-mobile', to: '/button' },
						{ label: 'Table', icon: 'pi pi-fw pi-table', to: '/table' },
						{ label: 'List', icon: 'pi pi-fw pi-list', to: '/list' },
						{ label: 'Tree', icon: 'pi pi-fw pi-share-alt', to: '/tree' },
						{ label: 'Panel', icon: 'pi pi-fw pi-tablet', to: '/panel' },
						{ label: 'Overlay', icon: 'pi pi-fw pi-clone', to: '/overlay' },
						{ label: 'Menu', icon: 'pi pi-fw pi-bars', to: '/menu' },
						{ label: 'Message', icon: 'pi pi-fw pi-comment', to: '/messages' },
						{ label: 'File', icon: 'pi pi-fw pi-file', to: '/file' },
						{ label: 'Chart', icon: 'pi pi-fw pi-chart-bar', to: '/chart' },
						{ label: 'Misc', icon: 'pi pi-fw pi-circle-off', to: '/misc' },
					]
				},
				{
					label: 'Pages', icon: 'pi pi-fw pi-clone',
					items: [
						{ label: 'Crud', icon: 'pi pi-fw pi-user-edit', to: '/crud' },
						{ label: 'Timeline', icon: 'pi pi-fw pi-calendar', to: '/timeline' },
						{ label: 'Empty', icon: 'pi pi-fw pi-circle-off', to: '/empty' }
					]
				},
				{
					label: 'Menu Hierarchy', icon: 'pi pi-fw pi-search',
					items: [
						{
							label: 'Submenu 1', icon: 'pi pi-fw pi-bookmark',
							items: [
								{
									label: 'Submenu 1.1', icon: 'pi pi-fw pi-bookmark',
									items: [
										{ label: 'Submenu 1.1.1', icon: 'pi pi-fw pi-bookmark' },
										{ label: 'Submenu 1.1.2', icon: 'pi pi-fw pi-bookmark' },
										{ label: 'Submenu 1.1.3', icon: 'pi pi-fw pi-bookmark' },
									]
								},
								{
									label: 'Submenu 1.2', icon: 'pi pi-fw pi-bookmark',
									items: [
										{ label: 'Submenu 1.2.1', icon: 'pi pi-fw pi-bookmark' },
										{ label: 'Submenu 1.2.2', icon: 'pi pi-fw pi-bookmark' }
									]
								},
							]
						},
						{
							label: 'Submenu 2', icon: 'pi pi-fw pi-bookmark',
							items: [
								{
									label: 'Submenu 2.1', icon: 'pi pi-fw pi-bookmark',
									items: [
										{ label: 'Submenu 2.1.1', icon: 'pi pi-fw pi-bookmark' },
										{ label: 'Submenu 2.1.2', icon: 'pi pi-fw pi-bookmark' },
										{ label: 'Submenu 2.1.3', icon: 'pi pi-fw pi-bookmark' },
									]
								},
								{
									label: 'Submenu 2.2', icon: 'pi pi-fw pi-bookmark',
									items: [
										{ label: 'Submenu 2.2.1', icon: 'pi pi-fw pi-bookmark' },
										{ label: 'Submenu 2.2.2', icon: 'pi pi-fw pi-bookmark' }
									]
								}
							]
						}
					]
				},
				{
					label: 'Get Started',
					items: [
						{ label: 'Documentation', icon: 'pi pi-fw pi-question', command: () => { window.location.hash = "#/documentation" } },
						{ label: 'View Source', icon: 'pi pi-fw pi-search', command: () => { window.location.href = "https://github.com/primefaces/sakai-vue" } }
					]
				}
			]

		watch(() => route, () => {
			toast.removeAllGroups()
		})

		const onWrapperClick = () => {
			if (!menuClick) {
				overlayMenuActive.value = false
				mobileMenuActive.value = false
			}

			menuClick = false;
		}

		const onMenuToggle = () => {
			menuClick = true;

			if (isDesktop()) {
				if (layoutMode.value === 'overlay') {
					if (mobileMenuActive.value === true) {
						overlayMenuActive.value = true
					}

					overlayMenuActive.value = !overlayMenuActive.value
					mobileMenuActive.value = false
				}
				else if (layoutMode.value === 'static') {
					staticMenuInactive.value = !staticMenuInactive.value
				}
			}
			else {
				mobileMenuActive.value = !mobileMenuActive.value
			}
		}
		const onSidebarClick = () => {
			menuClick = true
		}

		const onMenuItemClick = (event: any) => {
			if (event.item && !event.item.items) {
				overlayMenuActive.value = false
				mobileMenuActive.value = false
			}
		}

		const onLayoutChange = (mode: any) => {
			layoutMode.value = mode
		}

		const onLayoutColorChange = (mode: any) => {
			layoutColorMode.value = mode
		}

		const addClass = (element: Element, className: string) => {
			if (element.classList) {
				element.classList.add(className);
			} else {
				element.className += ' ' + className;
			}
		}

		const removeClass = (element: Element, className: string) => {
			if (element.classList) {
				element.classList.remove(className);
			} else {
				element.className = element.className.replace(new RegExp('(^|\\b)' + className.split(' ').join('|') + '(\\b|$)', 'gi'), ' ');
			}
		}

		const isDesktop = () => {
			return window.innerWidth >= 992;
		}

		const isSidebarVisible = () => {
			if (isDesktop()) {
				if (layoutMode.value === 'static') {
					return !staticMenuInactive.value
				} else if (layoutMode.value === 'overlay') {
					return overlayMenuActive.value
				}
			}

			return true;
		}

		const containerClass = computed(() => {
			return ['layout-wrapper', {
				'layout-overlay': layoutMode.value === 'overlay',
				'layout-static': layoutMode.value === 'static',
				'layout-static-sidebar-inactive': staticMenuInactive.value && layoutMode.value === 'static',
				'layout-overlay-sidebar-active': overlayMenuActive.value && layoutMode.value === 'overlay',
				'layout-mobile-sidebar-active': mobileMenuActive.value,
				'p-input-filled': primevue.config.inputStyle === 'filled',
				'p-ripple-disabled': primevue.config.ripple === false,
				'layout-theme-light': appState.theme.startsWith('saga')
			}]
		})

		const logo = computed(() => {
			return (layoutColorMode.value === 'dark') ? "images/logo-white.svg" : "images/logo.svg"
		})
		
		onBeforeUpdate(() => {
			if (mobileMenuActive.value) {
				addClass(document.body, 'body-overflow-hidden')
			} else {
				removeClass(document.body, 'body-overflow-hidden')
			}
		})

		return {
			layoutMode,
			layoutColorMode,
			mobileMenuActive,
			onLayoutChange,
			onLayoutColorChange,
			menu,
			containerClass,
			onSidebarClick,
			onMenuItemClick,
			onMenuToggle,
			onWrapperClick
		}
	}
})
</script>

<style lang="scss">
@import "./App.scss";
</style>
