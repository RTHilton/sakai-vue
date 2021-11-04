<template>
	<div ref="el" id="layout-config" :class="containerClass">
		<a href="#" class="layout-config-button" id="layout-config-button" @click="toggleConfigurator">
			<i class="pi pi-cog"></i>
		</a>
		<Button class="p-button-danger layout-config-close p-button-rounded p-button-text" icon="pi pi-times" @click="hideConfigurator"></Button>

		<div class="layout-config-content">
			<h6 class="mt-0">Component Scale</h6>
			<div class="config-scale">
				<Button icon="pi pi-minus" @click="decrementScale()" class="p-button-text" :disabled="scale === scales[0]"/>
				<i class="pi pi-circle-on" v-for="s of scales" :class="{'scale-active': s === scale}" :key="s"/>
				<Button icon="pi pi-plus"  @click="incrementScale()" class="p-button-text" :disabled="scale === scales[scales.length - 1]" />
			</div>

			<h6>Input Style</h6>
			<div class="p-formgroup-inline">
				<div class="field-radiobutton">
					<RadioButton id="input_outlined" name="inputstyle" value="outlined" :modelValue="primevue.config.inputStyle" @change="changeInputStyle('outlined')" />
					<label for="input_outlined">Outlined</label>
				</div>
				<div class="field-radiobutton">
					<RadioButton id="input_filled" name="inputstyle" value="filled" :modelValue="primevue.config.inputStyle" @change="changeInputStyle('filled')" />
					<label for="input_filled">Filled</label>
				</div>
			</div>

			<h6>Ripple Effect</h6>
			<InputSwitch :modelValue="rippleActive" @update:modelValue="changeRipple" />

			<h6>Menu Type</h6>
			<div class="p-formgroup-inline">
				<div class="field-radiobutton">
					<RadioButton id="static" name="layoutMode" value="static" v-model="d_layoutMode" @change="changeLayout($event, 'static')" />
					<label for="static">Static</label>
				</div>
				<div class="field-radiobutton">
					<RadioButton id="overlay" name="layoutMode" value="overlay" v-model="d_layoutMode" @change="changeLayout($event, 'overlay')" />
					<label for="overlay">Overlay</label>
				</div>
			</div>

			<h6>Color Scheme</h6>
			<div class="p-formgroup-inline">
				<div class="field-radiobutton">
					<RadioButton id="light" name="layoutColorMode" value="saga" v-model="themeScheme" @change="changeThemeScheme('saga')" />
					<label for="light">Light</label>
				</div>
				<div class="field-radiobutton">
					<RadioButton id="dim" name="layoutColorMode" value="vela" v-model="themeScheme" @change="changeThemeScheme('vela')" />
					<label for="dim">Dim</label>
				</div>
				<div class="field-radiobutton">
					<RadioButton id="dark" name="layoutColorMode" value="arya" v-model="themeScheme" @change="changeThemeScheme('arya')" />
					<label for="dark">Dark</label>
				</div>
			</div>

			<h6>Primary Color</h6>
			<div class="flex">
				<div style="width:2rem;height:2rem;border-radius:6px" class="bg-blue-500 mr-3 cursor-pointer" @click="changeThemeColor('blue')"></div>
				<div style="width:2rem;height:2rem;border-radius:6px" class="bg-green-500 mr-3 cursor-pointer" @click="changeThemeColor('green')"></div>
				<div style="width:2rem;height:2rem;border-radius:6px" class="bg-orange-500 mr-3 cursor-pointer" @click="changeThemeColor('orange')"></div>
				<div style="width:2rem;height:2rem;border-radius:6px" class="bg-purple-500 cursor-pointer" @click="changeThemeColor('purple')"></div>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
	import { defineComponent, computed, inject, watch, ref } from 'vue'
	import { useRoute } from 'vue-router'
	import { usePrimeVue } from 'primevue/config'

	export default defineComponent({
		props: {
			layoutMode: {
				type: String,
				default: null
			},
			layoutColorMode: {
				type: String,
				default: null
			}
		},
		setup(props, { emit }) {
			const appState = inject('$appState') as { theme: string, inputStyle: string }
			const primevue = usePrimeVue()
			const active = ref(false)
			const route = useRoute()
			const scale = ref(16)
			const scales = [12,13,14,15,16]
			const themeScheme = ref('saga')
			const themeColor = ref('blue')
			const d_layoutMode = ref(props.layoutMode)
			const d_layoutColorMode = ref(props.layoutColorMode)
			const el = ref(null as Element | null)

			let outsideClickListener: undefined | ((event: any) => void)

			const toggleConfigurator = (event: any) => {
				active.value = !active.value
				event.preventDefault()

				if (active.value) {
					bindOutsideClickListener()
				} else {
					unbindOutsideClickListener()
				}
			}
			const hideConfigurator = (event: any) => {
				active.value = false
				unbindOutsideClickListener()
				event.preventDefault()
			}

			const changeInputStyle = (value: string) => {
				primevue.config.inputStyle = value
			}

			const changeRipple = (value: boolean) => {
				primevue.config.ripple = value
			}

			const changeLayout = (event: any, layoutMode: any) => {
				emit('layout-change', layoutMode)
				event.preventDefault()
			}

			const changeLayoutColor = (event: any, layoutColor: any) => {
				emit('layout-color-change', layoutColor)
				event.preventDefault()
			}

			const bindOutsideClickListener = () => {
				if (!outsideClickListener) {
					outsideClickListener = (event) => {
						if (active.value && isOutsideClicked(event)) {
							active.value = false
						}
					}
					document.addEventListener('click', outsideClickListener)
				}
			}
			
			const unbindOutsideClickListener = () => {
				if (outsideClickListener) {
					document.removeEventListener('click', outsideClickListener)
					outsideClickListener = undefined
				}
			}

			const isOutsideClicked = (event: any) => {
				return !(el.value?.isSameNode(event.target) || el.value?.contains(event.target))
			}
			const decrementScale = () => {
				scale.value--
				applyScale()
			}

			const incrementScale = () => {
				scale.value++
				applyScale()
			}

			const applyScale = () => {
				document.documentElement.style.fontSize = scale.value + 'px'
			}

			const changeTheme = () => {
				let theme = themeScheme.value + '-' + themeColor.value
				let themeElement = document.getElementById('theme-link')
				themeElement?.setAttribute('href', themeElement?.getAttribute('href')?.replace(appState.theme, theme) ?? '')
				appState.theme = theme
			}

			const changeThemeScheme = (scheme: string) => {
				themeScheme.value = scheme
				changeTheme()
			}

			const changeThemeColor = (color: string) => {
				themeColor.value = color
				changeTheme()
			}

			watch (() => route, () => {
				if (active.value) {
					active.value = false
					unbindOutsideClickListener()
				}
			})

			watch (() => d_layoutMode, (curr) => {
				d_layoutMode.value = curr.value
			})

			watch (() => d_layoutColorMode, (curr) => {
				d_layoutColorMode.value = curr.value
			})

			const containerClass = computed(() => {
				return ['layout-config', {'layout-config-active': active.value}]
			})

			const rippleActive = computed(() => {
				return primevue.config.ripple
			})

			const inputStyle = computed(() => {
				return $appState.inputStyle
			})

			return {
				el,
				active,
				scales,
				scale,
				themeScheme,
				d_layoutMode,
				d_layoutColorMode,
				toggleConfigurator,
				hideConfigurator,
				changeInputStyle,
				changeRipple,
				changeLayout,
				changeLayoutColor,
				decrementScale,
				incrementScale,
				applyScale,
				changeTheme,
				changeThemeScheme,
				changeThemeColor,
				containerClass,
				rippleActive,
				inputStyle,
				primevue
			}
		}
	})
</script>
