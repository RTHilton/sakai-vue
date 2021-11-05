<template>
	<div class="layout-menu-container">
		<AppSubmenu :items="model" class="layout-menu" :root="true" @menuitem-click="onMenuItemClick" />
		<a href="https://www.primefaces.org/primeblocks-vue" class="block mt-3">
			<img alt="primeblocks" :src="bannerImage()" class="w-full" />
		</a>
	</div>
</template>

<script lang="ts">
	import { defineComponent, computed, inject } from 'vue'
	import AppSubmenu from './AppSubmenu.vue'

	export default defineComponent({
		props: {
			model: Array
		},
		components: {
			AppSubmenu
		},
		setup(props, { emit }) {
			const appState = inject('$appState') as { theme: string, inputStyle: string, darkTheme: boolean }

			const onMenuItemClick = (event: any) => {
				emit('menuitem-click', event)
			}

			const bannerImage = () => {
				return appState.darkTheme ? 'images/banner-primeblocks-dark.png' : 'images/banner-primeblocks.png'
			}

			const darkTheme = computed(() => {
				return appState.darkTheme
			})

			return {
				onMenuItemClick,
				darkTheme,
				bannerImage
			}
		}
	})
</script>

<style scoped>

</style>