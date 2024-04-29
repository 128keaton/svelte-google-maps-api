<script lang="ts">
	const infoWindow = new google.maps.InfoWindow();

	export let contentString: string;

	$: infoWindow.setContent(contentString);

	export function openInfoWindow(anchor: google.maps.marker.AdvancedMarkerElement, map: google.maps.Map) {
		infoWindow.open({
			anchor,
			map
		});
	}
</script>
