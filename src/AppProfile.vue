<template>
	<div class="layout-profile">
		<div>
			<img src="images/profile.png" alt="profile image" />
		</div>
		<button class="p-link layout-profile-link" @click="onClick">
			<span class="username">Claire Williams</span>
			<i class="pi pi-fw pi-cog"></i>
		</button>
		<transition name="layout-submenu-wrapper">
			<ul v-show="expanded">
				<li>
					<button class="p-link">
						<i class="pi pi-fw pi-user"></i>
						<span>Account</span>
					</button>
				</li>
				<li>
					<button class="p-link">
						<i class="pi pi-fw pi-inbox"></i>
						<span>Notifications</span>
						<span class="menuitem-badge">2</span>
					</button>
				</li>
				<li>
					<button class="p-link">
						<i class="pi pi-fw pi-power-off"></i>
						<span>Logout</span>
					</button>
				</li>
			</ul>
		</transition>
	</div>
</template>

<script lang="ts">
	import { defineComponent, ref } from 'vue'
	export default defineComponent({
		name: 'AppProfile',
		setup() {
			const expanded = ref(false)

			const onClick = (event: Event) => {
				expanded.value = !expanded.value
				event.preventDefault()
			}

			return {
				expanded,
				onClick
			}
		}
	})
</script>

<style scoped>
</style>