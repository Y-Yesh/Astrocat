<script lang="ts">
	import { LabeledInput } from '$lib/components/labeled/index.js';
	import { Button } from '$lib/components/ui/button/index.js';
	import { Input } from '$lib/components/ui/input/index.js';
	import { Label } from '$lib/components/ui/label/index.js';
	import { Separator } from '$lib/components/ui/separator/index.js';
	import { getRecorderFromContext } from '$lib/query/singletons/recorder';
	import {
		getRegisterShortcutsFromContext,
		settings,
	} from '$lib/stores/settings.svelte';

	const recorder = getRecorderFromContext();
	const registerShortcuts = getRegisterShortcutsFromContext();
</script>

<svelte:head>
	<title>Configure Shortcuts - Astrocat</title>
</svelte:head>

<div class="space-y-6 bg-white text-gray-900 p-6 rounded-lg">
	<div>
		<h3 class="text-lg font-medium">Shortcuts</h3>
		<p class="text-muted-foreground text-sm">
			Configure your shortcuts for activating Astrocat.
		</p>
	</div>
	<Separator />

	<LabeledInput
		id="local-shortcut"
		label="Local Shortcut"
		placeholder="Local Shortcut to toggle recording"
		value={settings.value['shortcuts.currentLocalShortcut']}
		onchange={({ currentTarget: { value } }) => {
			settings.value = {
				...settings.value,
				'shortcuts.currentLocalShortcut': value,
			};
			registerShortcuts.registerLocalShortcut({
				shortcut: value,
				callback: () => recorder.toggleRecording(),
			});
		}}
	/>

	{#if window.__TAURI_INTERNALS__}
		<LabeledInput
			id="global-shortcut"
			label="Global Shortcut"
			placeholder="Global Shortcut to toggle recording"
			value={settings.value['shortcuts.currentGlobalShortcut']}
			onchange={({ currentTarget: { value } }) => {
				settings.value = {
					...settings.value,
					'shortcuts.currentGlobalShortcut': value,
				};
				registerShortcuts.registerGlobalShortcut({
					shortcut: value,
					callback: () => recorder.toggleRecording(),
				});
			}}
		/>
	{:else}
		<Label class="text-sm" for="global-shortcut">Global Shortcut</Label>
		<div class="relative">
			<Input
				id="global-shortcut"
				placeholder="Global Shortcut to toggle recording"
				value={settings.value['shortcuts.currentGlobalShortcut']}
				type="text"
				autocomplete="off"
				disabled
			/>
			<Button
				class="absolute inset-0 backdrop-blur"
				href="/global-shortcut"
				variant="link"
			>
				Enable Global Shortcut
			</Button>
		</div>
	{/if}
</div>