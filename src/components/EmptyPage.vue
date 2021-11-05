<template>
	<div class="grid">
		<div class="col-12">
			<div class="card">
				<h5>Empty Page</h5>
				<p>Use this page to start from scratch and place your custom content.</p>
				<div class="main-panel">
					<div class="editor-panel">
						<ace-editor
							class="editor"
							:value="panel1"
							lang="json"
							theme="twilight"
							:printMargin="false"
						/>
						<ace-editor
							class="editor"
							:value="panel2"
							lang="json"
							theme="twilight"
							:printMargin="false"
						/>
					</div>
					<div class="results-panel">
						<ace-editor
							class="editor"
							:value="results"
							lang="json"
							theme="twilight"
							:printMargin="false"
						/>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
	import { defineComponent, ref } from 'vue'
	export default defineComponent({
		name: 'EmptyPage',
		setup() {
			const panel1 = ref('')
			const panel2 = ref('')
			const results = ref('')
			return {
				panel1,
				panel2,
				results
			}
		}
	})
</script>

<style scoped lang="scss">
.main-panel {
	display: flex;
	flex-direction: column;
	width: 100%;

	.editor-panel {
		display: flex;
		flex-direction: row;
		border-bottom: 1px solid white;
		box-sizing: border-box;
		padding: 2rem;

		.editor {
			display: flex;
			width: 50%;
			height: 400px;
			border-left: 1px solid white;
			border-right: 1px solid white;
		}
	}
	.results-panel {
		display: flex;

		.editor {
			width: 100%;
			height: 300px;
		}
	}
}
</style>
