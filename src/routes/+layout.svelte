<script lang="ts">
	import { apiKey, mapId } from '../keyStore';

	let currentApiKey: string = $apiKey;
	let currentMapId: string = $mapId;
</script>
<svelte:head>
	<title>Svelte Google Maps API</title>
</svelte:head>

<div class="links">

	<ul>
		<li>
			<a href="/map1">Map 1</a>
		</li>
		<li>
			<a href="/map2">Map 2</a>
		</li>
	</ul>

</div>

<div class="content">
	<input type="text" placeholder="api key" bind:value={currentApiKey} />
	<input type="text" placeholder="map ID" bind:value={currentMapId}/>


	<slot/>

</div>
