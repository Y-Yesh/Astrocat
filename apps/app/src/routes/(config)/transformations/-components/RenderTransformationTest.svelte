<script lang="ts">
	import { LabeledTextarea } from '$lib/components/labeled/index.js';
	import { Button } from '$lib/components/ui/button';
	import * as Card from '$lib/components/ui/card';
	import { getTransformerFromContext } from '$lib/query/singletons/transformer';
	import type { Transformation } from '$lib/services/db';
	import { Loader2Icon, PlayIcon } from 'lucide-svelte';
	import { nanoid } from 'nanoid/non-secure';

	let { transformation }: { transformation: Transformation } = $props();

	let input = $state('');
	let output = $state('');

	const transformer = getTransformerFromContext();
</script>

<Card.Header>
	<Card.Title>Test Transformation</Card.Title>
	<Card.Description>
		Try out your transformation with sample input
	</Card.Description>
</Card.Header>
<Card.Content class="space-y-6">
	<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
		<LabeledTextarea
			id="input"
			label="Input Text"
			bind:value={input}
			placeholder="Enter text to transform..."
			rows={5}
		/>

		<LabeledTextarea
			id="output"
			label="Output Text"
			value={output}
			placeholder="Transformed text will appear here..."
			rows={5}
			readonly
		/>
	</div>

	<Button
		onclick={() =>
			transformer.transformInput.mutate(
				{ input, transformationId: transformation.id, toastId: nanoid() },
				{ onSuccess: (o) => (output = o) },
			)}
		disabled={transformer.transformInput.isPending ||
			!input.trim() ||
			transformation.steps.length === 0}
		class="w-full"
	>
		{#if transformer.transformInput.isPending}
			<Loader2Icon class="mr-2 h-4 w-4 animate-spin" />
		{:else}
			<PlayIcon class="mr-2 h-4 w-4" />
		{/if}
		{transformer.transformInput.isPending
			? 'Running Transformation...'
			: 'Run Transformation'}
	</Button>
</Card.Content>
