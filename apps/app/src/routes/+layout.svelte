<script lang="ts">
	import { onNavigate } from '$app/navigation';
	import { SvelteQueryDevtools } from '@tanstack/svelte-query-devtools';
	import '../app.css';
	import { queryClient } from '$lib/query';
	import { QueryClientProvider } from '@tanstack/svelte-query';
	import AppShell from './+layout/AppShell.svelte';
	import GlobalSingletonsContext from './+layout/GlobalSingletonsContext.svelte';

	let { children } = $props();

	onNavigate((navigation) => {
		if (!document.startViewTransition) return;

		return new Promise((resolve) => {
			document.startViewTransition(async () => {
				resolve();
				await navigation.complete;
			});
		});
	});
</script>

<svelte:head>
	<title>Astrocat</title>
</svelte:head>

<QueryClientProvider client={queryClient}>
	<GlobalSingletonsContext>
		<AppShell>
			{@render children()}
		</AppShell>
	</GlobalSingletonsContext>
	<SvelteQueryDevtools initialIsOpen={false} buttonPosition="bottom-left" />
</QueryClientProvider>
