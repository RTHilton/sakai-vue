<template>
	<div class="layout-menu-container">
		<AppSubmenu :items="model" class="layout-menu" :root="true" @menuitem-click="onMenuItemClick" />
		<a href="https://www.primefaces.org/primeblocks-vue" class="block mt-3">
			<img alt="primeblocks" :src="darkTheme ? 'images/banner-primeblocks.png' : 'images/banner-primeblocks-dark.png'" class="w-full" />
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
			const appState = inject('$appState') as { theme: string, inputStyle: string }

			const onMenuItemClick = (event: any) => {
				emit('menuitem-click', event)
			}

			const darkTheme = computed(() => {
				return appState.theme.startsWith('saga')
			})

			return {
				onMenuItemClick,
				darkTheme
			}
		}
	})
</script>

<style scoped>

</style>