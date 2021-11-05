<template>
	<div class="layout-footer">
		<img alt="Logo" :src="footerImage()" height="20" class="mr-2" />
		by
		<span class="font-medium ml-2">PrimeVue</span>
	</div>
</template>

<script lang="ts">
	import { defineComponent, computed, inject } from 'vue'

	export default defineComponent({
		name: 'AppFooter',
		setup() {
			const appState = inject('$appState') as { theme: string, inputStyle: string, darkTheme: boolean }

			const footerImage = () => {
				return appState.darkTheme ? 'images/logo-white.svg' : 'images/logo-dark.svg'
			}

			return {
				footerImage
			}
		}
	})
</script>

<style scoped>

</style>