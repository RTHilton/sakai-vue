<template>
	<div ref="el" id="layout-config" :class="containerClass">
		<a href="#" class="layout-config-button" id="layout-config-button" @click="toggleConfigurator">
			<i class="pi pi-cog"></i>
		</a>
		<Button class="p-button-danger layout-config-close p-button-rounded p-button-text" icon="pi pi-times" @click="hideConfigurator" :style="{'z-index': 1}"></Button>

		<div class="layout-config-content">
			<h5 class="mt-0">Component Scale</h5>
			<div class="config-scale">
				<Button icon="pi pi-minus" @click="decrementScale()" class="p-button-text" :disabled="scale === scales[0]"/>
				<i class="pi pi-circle-on" v-for="s of scales" :class="{'scale-active': s === scale}" :key="s"/>
				<Button icon="pi pi-plus"  @click="incrementScale()" class="p-button-text" :disabled="scale === scales[scales.length - 1]" />
			</div>

			<h5>Input Style</h5>
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

			<h5>Ripple Effect</h5>
			<InputSwitch :modelValue="rippleActive" @update:modelValue="changeRipple" />

			<h5>Menu Type</h5>
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

			<h5>Themes</h5>
			<h6 class="mt-0">Bootstrap</h6>
			<div class="grid free-themes">
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'bootstrap4-light-blue')">
						<img src="/images/themes/bootstrap4-light-blue.svg" alt="Bootstrap Light Blue" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'bootstrap4-light-purple')">
						<img src="/images/themes/bootstrap4-light-purple.svg" alt="Bootstrap Light Blue" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'bootstrap4-dark-blue', true)">
						<img src="/images/themes/bootstrap4-dark-blue.svg" alt="Bootstrap Dark Blue" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'bootstrap4-dark-purple', true)">
						<img src="/images/themes/bootstrap4-dark-purple.svg" alt="Bootstrap Dark Blue" />
					</button>
				</div>
			</div>

			<h6>Material Design</h6>
			<div class="grid free-themes">
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'md-light-indigo')">
						<img src="/images/themes/md-light-indigo.svg" alt="Material Light Indigo" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'md-light-deeppurple')">
						<img src="/images/themes/md-light-deeppurple.svg" alt="Material Light Deep Purple" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'md-dark-indigo', true)">
						<img src="/images/themes/md-dark-indigo.svg" alt="Material Dark Indigo" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'md-dark-deeppurple', true)">
						<img src="/images/themes/md-dark-deeppurple.svg" alt="Material Dark Deep Purple" />
					</button>
				</div>
			</div>

			<h6>Material Design Compact</h6>
			<div class="grid free-themes">
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'mdc-light-indigo')">
						<img src="/images/themes/md-light-indigo.svg" alt="Material Compact Light Indigo"/>
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'mdc-light-deeppurple')">
						<img src="/images/themes/md-light-deeppurple.svg" alt="Material Compact Deep Purple" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'mdc-dark-indigo', true)">
						<img src="/images/themes/md-dark-indigo.svg" alt="Material Compact Dark Indigo" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'mdc-dark-deeppurple', true)">
						<img src="/images/themes/md-dark-deeppurple.svg" alt="Material Compact Dark Deep Purple" />
					</button>
				</div>
			</div>

			<h6>Tailwind</h6>
			<div class="grid free-themes">
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'tailwind-light')">
						<img src="/images/themes/tailwind-light.png" alt="Tailwind Light"/>
					</button>
				</div>
			</div>

			<h6>Fluent UI</h6>
			<div class="grid free-themes">
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'fluent-light')">
						<img src="/images/themes/fluent-light.png" alt="Fluent Light"/>
					</button>
				</div>
			</div>

			<h6>PrimeOne Design</h6>
			<div class="grid free-themes">
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'saga-blue')">
						<img src="/images/themes/saga-blue.png" alt="Saga Blue" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'saga-green')">
						<img src="/images/themes/saga-green.png" alt="Saga Green" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'saga-orange')">
						<img src="/images/themes/saga-orange.png" alt="Saga Orange" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'saga-purple')">
						<img src="/images/themes/saga-purple.png" alt="Saga Purple" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'vela-blue', true)">
						<img src="/images/themes/vela-blue.png" alt="Vela Blue" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'vela-green', true)">
						<img src="/images/themes/vela-green.png" alt="Vela Green" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'vela-orange', true)">
						<img src="/images/themes/vela-orange.png" alt="Vela Orange" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'vela-purple', true)">
						<img src="/images/themes/vela-purple.png" alt="Vela Purple" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'arya-blue', true)">
						<img src="/images/themes/arya-blue.png" alt="Arya Blue" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'arya-green', true)">
						<img src="/images/themes/arya-green.png" alt="Arya Green" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'arya-orange', true)">
						<img src="/images/themes/arya-orange.png" alt="Arya Orange" />
					</button>
				</div>
				<div class="col-3 text-center">
					<button class="p-link" type="button" @click="changeTheme($event, 'arya-purple', true)">
						<img src="/images/themes/arya-purple.png" alt="Arya Purple" />
					</button>
				</div>
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
			const appState = inject('$appState') as { theme: string, inputStyle: string, darkTheme: boolean }
			const primevue = usePrimeVue()
			const active = ref(false)
			const route = useRoute()
			const scale = ref(16)
			const scales = [12,13,14,15,16]
			const themeScheme = ref('arya-blue')
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

			const changeTheme = (event: any, theme: string, darkMode = false) => {
				let themeElement = document.getElementById('theme-link')
				themeElement?.setAttribute('href', themeElement?.getAttribute('href')?.replace(appState.theme, theme) ?? '')
				appState.theme = theme
				appState.darkTheme = darkMode
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
				return appState.inputStyle
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
				containerClass,
				rippleActive,
				inputStyle,
				primevue
			}
		}
	})
</script>
