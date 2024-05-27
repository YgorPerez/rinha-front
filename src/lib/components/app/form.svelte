<script lang="ts">
	import { buttonVariants } from '$lib/components/ui/button';
	import Input from '$lib/components/ui/input/input.svelte';
	import Label from '$lib/components/ui/label/label.svelte';

	export let highlightedJson: string;
	export let fileName: string;

	function handleFile(e: Event) {
		highlightedJson = ''
		const target = e.target as HTMLInputElement;
		const file = target.files?.[0];
		if (!file) return;
		fileName = file.name;
		const reader = file.stream().getReader();
		const decoder = new TextDecoder();
		const work = (reader: ReadableStreamDefaultReader<Uint8Array>) => {
			reader.read().then(({ done, value }) => {
				if (!done) {
					let stringValue = decoder.decode(value);
					if (!stringValue) return;
					if (stringValue.endsWith(',')) {
						stringValue = stringValue.substr(0, stringValue.length - 1);
					}

					try {
						console.log(`Parsing chunk: ${stringValue}`);
						highlightedJson += stringValue;
					} catch (error) {
						console.log(`Failed to parse chunk. Error: ${error}`);
					}

					work(reader);
				}
			});
		};
		work(reader);
	}
</script>

<div>
	<Label for="file" class={`${buttonVariants()} cursor-pointer font-medium`}>Load JSON</Label>
	<Input type="file" accept=".json" id="file" class="hidden" on:change={handleFile} />
</div>
