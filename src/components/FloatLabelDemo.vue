<template>
	<div class="floatlabel-demo">
		<div class="card">
			<h5>Float Label</h5>
			<div class="grid p-fluid mt-3">
				<div class="field col-12 md:col-4">	
					<span class="p-float-label">
						<InputText type="text" id="inputtext" v-model="value1" />
						<label for="inputtext">InputText</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<AutoComplete id="autocomplete" v-model="value2" :suggestions="filteredCountries" @complete="searchCountry($event)" field="name"></AutoComplete>
						<label for="autocomplete">AutoComplete</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<Calendar inputId="calendar" v-model="value3"></Calendar>
						<label for="calendar">Calendar</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<Chips inputId="chips" v-model="value4"></Chips>
						<label for="chips">Chips</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<InputMask id="inputmask" mask="99/99/9999" v-model="value5"></InputMask>
						<label for="inputmask">InputMask</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<InputNumber id="inputnumber" v-model="value6"></InputNumber>
						<label for="inputnumber">InputNumber</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<div class="p-inputgroup">
						<span class="p-inputgroup-addon">
							<i class="pi pi-user"></i>
						</span>
						<span class="p-float-label">
							<InputText type="text" id="inputgroup" v-model="value7" />
							<label for="inputgroup">InputGroup</label>
						</span>
					</div>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<Dropdown id="dropdown" :options="cities" v-model="value8" optionLabel="name"></Dropdown>
						<label for="dropdown">Dropdown</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<MultiSelect id="multiselect" :options="cities" v-model="value9" optionLabel="name" :filter="false"></MultiSelect>
						<label for="multiselect">MultiSelect</label>
					</span>
				</div>
				<div class="field col-12 md:col-4">
					<span class="p-float-label">
						<Textarea inputId="textarea" rows="3" cols="30" v-model="value10"></Textarea>
						<label for="textarea">Textarea</label>
					</span>
				</div>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
	import { defineComponent, ref, onMounted } from 'vue'
	import CountryService from '@/service/CountryService'
	
	export default defineComponent({
		name: 'FloatLabelDemo',
		setup() {
			const countryService = new CountryService()

			const countries = ref([] as any[])
			const filteredCountries = ref([] as any[])
			const cities = [
				{ name: 'New York', code: 'NY' },
				{ name: 'Rome', code: 'RM' },
				{ name: 'London', code: 'LDN' },
				{ name: 'Istanbul', code: 'IST' },
				{ name: 'Paris', code: 'PRS' },
			]
			const value1 = ref(null)
			const value2 = ref(null)
			const value3 = ref(null)
			const value4 = ref(null)
			const value5 = ref(null)
			const value6 = ref(null)
			const value7 = ref(null)
			const value8 = ref(null)
			const value9 = ref(null)
			const value10 = ref(null)

			const searchCountry = (event: any) => {
				const query = event.query
				filteredCountries.value = countries.value.filter((country) => {
					return country.name.toLowerCase().indexOf(query.toLowerCase()) == 0
				})
			}

			onMounted(() => {
				countryService.getCountries().then((data) => {
					countries.value = data
				})
			})

			return {
				countries,
				filteredCountries,
				cities,
				value1,
				value2,
				value3,
				value4,
				value5,
				value6,
				value7,
				value8,
				value9,
				value10,
				searchCountry
			}
		}
	})
</script>
