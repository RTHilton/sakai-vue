<template>
	<div class="grid messages-demo">
		<div class="col-12 lg:col-6">
			<div class="card">
				<h5>Toast</h5>

				<Toast />
				<Button @click="showSuccess()" label="Success" class="p-button-success mr-2" />
				<Button @click="showInfo()" label="Info" class="p-button-info mr-2" />
				<Button @click="showWarn()" label="Warn" class="p-button-warning mr-2" />
				<Button @click="showError()" label="Error" class="p-button-danger mr-2" />
			</div>
		</div>

		<div class="col-12 lg:col-6">
			<div class="card">
				<h5>Messages</h5>

				<Button label="Success" @click="addSuccessMessage()" class="p-button-success mr-2"/>
				<Button label="Info" @click="addInfoMessage()" class="p-button-info mr-2"/>
				<Button label="Warn" @click="addWarnMessage()" class="p-button-warning mr-2"/>
				<Button label="Error" @click="addErrorMessage()" class="p-button-danger mr-2"/>

				<transition-group name="p-messages" tag="div">
					<Message v-for="msg of message" :severity="msg.severity" :key="msg.content">{{msg.content}}</Message>
				</transition-group>
			</div>
		</div>

		<div class="col-12 lg:col-8">
			<div class="card">
				<h5>Inline</h5>
				<div class="field grid">
					<label for="username1" class="col-fixed w-9rem">Username</label>
					<div class="col">
						<InputText id="username1" v-model="username" :required="true" class="p-invalid mr-2"></InputText>
						<InlineMessage>Username is required</InlineMessage>
					</div>
				</div>
				<div class="field grid">
					<label for="email" class="col-fixed w-9rem">Email</label>
					<div class="col">
						<InputText id="email" v-model="email" :required="true" class="p-invalid mr-2"></InputText>
						<InlineMessage/>
					</div>
				</div>
			</div>
		</div>

		<div class="col-12 lg:col-4">
			<div class="card">
				<h5>Help Text</h5>
				<div class="field p-fluid">
					<label for="username2">Username</label>
					<InputText id="username2" type="username" class="p-error" aria-describedby="username-help" />
					<small id="username-help" class="p-error">Enter your username to reset your password.</small>
				</div>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
	import { defineComponent, ref } from 'vue'
	import { useToast } from 'primevue/usetoast'

	export default defineComponent({
		name: 'MessagesDemo',
		setup() {
			const toast = useToast()

			const message = ref([] as any[])
			const username = ref(null)
			const email = ref(null)

			const addSuccessMessage = () => {
				message.value = [{severity: 'success', content: 'Message Detail'}]
			}

			const addInfoMessage = () => {
				message.value = [{severity: 'info', content: 'Message Detail'}]
			}

			const addWarnMessage = () => {
				message.value = [{severity: 'warn', content: 'Message Detail'}]
			}

			const addErrorMessage = () => {
				message.value = [{severity: 'error', content: 'Message Detail'}]
			}

			const showSuccess = () => {
				toast.add({severity:'success', summary: 'Success Message', detail:'Message Detail', life: 3000});
			}

			const showInfo = () => {
				toast.add({severity:'info', summary: 'Info Message', detail:'Message Detail', life: 3000});
			}

			const showWarn = () => {
				toast.add({severity:'warn', summary: 'Warn Message', detail:'Message Detail', life: 3000});
			}

			const showError = () => {
				toast.add({severity:'error', summary: 'Error Message', detail:'Message Detail', life: 3000});
			}

			return {
				message,
				username,
				email,
				addSuccessMessage,
				addInfoMessage,
				addWarnMessage,
				addErrorMessage,
				showSuccess,
				showInfo,
				showWarn,
				showError
			}
		}
	})
</script>
