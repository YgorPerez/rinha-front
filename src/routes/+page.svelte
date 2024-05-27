<script lang="ts">
	import Form from '$lib/components/app/form.svelte';
	import Button from '$lib/components/ui/form/form-button.svelte';

	let highlightedJson: string;
	let fileName: string;
</script>

{#if highlightedJson}
	<div class="flex w-full justify-center">
		<div class="space-y-3 md:w-[600px] md:max-w-[90%]">
			<h1 class="text-3xl font-bold">{fileName}</h1>
			<pre>{highlightedJson}</pre>
			<div>
				<Button on:click={() => (highlightedJson = '')}>Go back</Button>
			</div>
		</div>
	</div>
{:else}
	<div
		class="absolute left-1/2 top-1/2 flex w-full -translate-x-1/2 -translate-y-1/2 flex-col space-y-6 text-center"
	>
		<h1 class="text-5xl font-bold">JSON Tree Viewer</h1>
		<p class="text-balance text-2xl">
			Simple JSON Viewer that runs completely on-client. No data exchange
		</p>
		<Form bind:highlightedJson bind:fileName />
	</div>
{/if}
