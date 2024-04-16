<script lang="ts">
	import type { Library } from '$lib/types/googleMap.js';
	import { BROWSER as browser } from 'esm-env';
	import { getContext, setContext } from 'svelte';

	export let apiKey = '';
	export let libraries: Library[] = ['places'];

	let loaded = false;
	let isReady = false;

	const initialize = (key: string) => {
		if (getContext<{isReady?: boolean}>('googleMap')?.isReady)
			return;

		if (!key || key === '') {
			return;
		}
		// @ts-ignore
		window.svelteGoogleMapInit = () => {
			loaded = true;
		};
		const url = `//maps.googleapis.com/maps/api/js?${key ? `key=${key}&` : ''}${
			libraries.length > 0 ? `libraries=${libraries.sort().join(',')}&` : ''
		}callback=svelteGoogleMapInit&loading=async`;
		const script = document.createElement('script');
		script.type = 'text/javascript';
		script.src = url;
		script.async = true;
		document.head.appendChild(script);
	};
	$: if (browser) {
		initialize(apiKey);
	}
	$: if (loaded) {
		setContext('googleMap', {
			isReady: true
		});
		isReady = true;
	}
</script>

{#if isReady}
	<slot />
{/if}
