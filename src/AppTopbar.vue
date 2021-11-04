<template>
	<div class="layout-topbar">
		<router-link to="/" class="layout-topbar-logo">
			<img alt="Logo" :src="darkTheme ? 'images/logo-dark.svg' : 'images/logo-white.svg'" />
			<span>SAKAI</span>
		</router-link>
		<button class="p-link layout-menu-button layout-topbar-button" @click="onMenuToggle">
			<i class="pi pi-bars"></i>
		</button>

		<button
			class="p-link layout-topbar-menu-button layout-topbar-button"
			v-styleclass="{
				selector: '@next', enterClass: 'hidden', enterActiveClass: 'scalein',
				leaveToClass: 'hidden', leaveActiveClass: 'fadeout', hideOnOutsideClick: true
			}"
		>
			<i class="pi pi-ellipsis-v"></i>
		</button>
		<ul class="layout-topbar-menu hidden lg:flex origin-top">
			<li>
				<button class="p-link layout-topbar-button">
					<i class="pi pi-calendar"></i>
					<span>Events</span>
				</button>
			</li>
			<li>
				<button class="p-link layout-topbar-button">
					<i class="pi pi-cog"></i>
					<span>Settings</span>
				</button>
			</li>
			<li>
				<button class="p-link layout-topbar-button">
					<i class="pi pi-user"></i>
					<span>Profile</span>
				</button>
			</li>
		</ul>
	</div>
</template>

<script lang="ts">
import { defineComponent, computed, inject } from 'vue'
export default defineComponent({
	name: 'AppTopbar',
	setup(_, { emit }) {
		const appState = inject('$appState') as { theme: string, inputStyle: string }
		
		const onMenuToggle = (event: any) => {
			emit('menu-toggle', event)
		}
		
		const onTopbarMenuToggle = (event: any) => {
			emit('topbar-menu-toggle', event)
		}
		
		const darkTheme = computed(() => {
			return appState.theme.startsWith('saga')
		})

		return {
			onMenuToggle,
			onTopbarMenuToggle,
			darkTheme
		}
	}
})
</script>