<template>
	<div class="layout-footer">
		<img alt="Logo" :src="darkTheme ? 'images/logo-dark.svg' : 'images/logo-white.svg'" height="20" class="mr-2" />
		by
		<span class="font-medium ml-2">PrimeVue</span>
	</div>
</template>

<script lang="ts">
	import { defineComponent, computed, inject } from 'vue'

	export default defineComponent({
		name: 'AppFooter',
		setup() {
			const appState = inject('$appState') as { theme: string, inputStyle: string }

			const darkTheme = computed(() => {
				return appState.theme.startsWith('saga');
			})

			return {
				darkTheme
			}
		}
	})
</script>

<style scoped>

</style>