<script lang="ts">
	import WhisperingButton from '$lib/components/WhisperingButton.svelte';
	import { GithubIcon } from '$lib/components/icons';
	import { settings } from '$lib/stores/settings.svelte';
	import { cn } from '$lib/utils';
	import { LogicalSize, getCurrentWindow } from '@tauri-apps/api/window';
	import {
		LayersIcon,
		ListIcon,
		Minimize2Icon,
		MoonIcon,
		SettingsIcon,
		SunIcon,
	} from 'lucide-svelte';
	import { toggleMode } from 'mode-watcher';

	let { class: className }: { class?: string } = $props();
</script>

<nav
	class={cn('flex items-center gap-1.5', className)}
	style="view-transition-name: nav"
>
	<WhisperingButton
		tooltipContent="Recordings"
		href="/recordings"
		variant="ghost"
		size="icon"
	>
		<ListIcon class="h-4 w-4" aria-hidden="true" />
	</WhisperingButton>
	<WhisperingButton
		tooltipContent="Transformations"
		href="/transformations"
		variant="ghost"
		size="icon"
		class="relative"
	>
		<LayersIcon class="h-4 w-4" aria-hidden="true" />
	</WhisperingButton>
	<WhisperingButton
		tooltipContent="Settings"
		href="/settings"
		variant="ghost"
		size="icon"
	>
		<SettingsIcon class="h-4 w-4" aria-hidden="true" />
	</WhisperingButton>


	{#if window.__TAURI_INTERNALS__}
		<WhisperingButton
			tooltipContent="Minimize"
			onclick={() => getCurrentWindow().setSize(new LogicalSize(72, 84))}
			variant="ghost"
			size="icon"
		>
			<Minimize2Icon class="h-4 w-4" aria-hidden="true" />
		</WhisperingButton>
	{/if}
</nav>

<style>
	@keyframes ping {
		75%,
		100% {
			transform: scale(2);
			opacity: 0;
		}
	}

	.animate-ping {
		animation: ping 1s cubic-bezier(0, 0, 0.2, 1) infinite;
	}
</style>

