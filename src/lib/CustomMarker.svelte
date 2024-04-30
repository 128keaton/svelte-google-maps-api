<script lang="ts">
	import { createEventDispatcher, getContext, onDestroy, onMount } from 'svelte';
	import { BROWSER as browser } from 'esm-env';

	const dispatch = createEventDispatcher();

	export let position: google.maps.LatLng | google.maps.LatLngLiteral;
	export let options: google.maps.marker.AdvancedMarkerElementOptions = {};
	export let customClassName = 'custom-marker';
	export let infoWindowContent: string | Element | Text | null = null;
	export let useInfoWindow = false;

	const onClick: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		dispatch('click', event);

		if (useInfoWindow && infoWindow && browser && marker && map) {
			infoWindow.open({
				anchor: marker,
				map,
			});
		}
	};

	const onClickableChanged: () => void = () => {
		dispatch('clickableChanged');
	};

	const onCursorChanged: () => void = () => {
		dispatch('cursorChanged');
	};

	const onAnimationChanged: () => void = () => {
		dispatch('animationChanged');
	};

	const onDoubleClick: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{doubleClick: google.maps.MapMouseEvent}>()('doubleClick', event);
	};

	const onDrag: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{drag: google.maps.MapMouseEvent}>()('drag', event);
	};

	const onDragEnd: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{dragEnd: google.maps.MapMouseEvent}>()('dragEnd', event);
	};

	const onDraggableChanged: () => void = () => {
		dispatch('draggableChanged');
	};


	const onDragStart: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{dragStart: google.maps.MapMouseEvent}>()('dragStart', event);
	};

	const onFlatChanged: () => void = () => {
		dispatch('flatChanged');
	};

	const onIconChanged: () => void = () => {
		dispatch('iconChanged');
	};

	const onMouseDown: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{mouseDown: google.maps.MapMouseEvent}>()('mouseDown', event);
	};

	const onMouseOut: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{mouseOut: google.maps.MapMouseEvent}>()('mouseOut', event);
	};

	const onMouseOver: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{mouseOver: google.maps.MapMouseEvent}>()('mouseOver', event);
	};

	const onMouseUp: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{mouseUp: google.maps.MapMouseEvent}>()('mouseUp', event);
	};

	const onPositionChanged: () => void = () => {
		dispatch('positionChanged');
	};

	const onRightClick: (event: google.maps.MapMouseEvent) => void = (event: google.maps.MapMouseEvent) => {
		createEventDispatcher<{rightClick: google.maps.MapMouseEvent}>()('rightClick', event);
	};

	const onShapeChanged: () => void = () => {
		dispatch('shapeChanged');
	};

	const onTitleChanged: () => void = () => {
		dispatch('titleChanged');
	};

	const onVisibleChanged: () => void = () => {
		dispatch('visibleChanged');
	};

	const onZindexChanged: () => void = () => {
		dispatch('zIndexChanged');
	};

	const onLoad: (marker: google.maps.marker.AdvancedMarkerElement) => void = (marker: google.maps.marker.AdvancedMarkerElement) => {
		createEventDispatcher<{load: google.maps.marker.AdvancedMarkerElement}>()('load', marker);
	};

	const onUnmount: (marker: google.maps.marker.AdvancedMarkerElement) => void = (marker: google.maps.marker.AdvancedMarkerElement) => {
		createEventDispatcher<{unmount: google.maps.marker.AdvancedMarkerElement}>()('unmount', marker);
	};


	let clickListener: google.maps.MapsEventListener | null = null;
	let clickableChangedListener: google.maps.MapsEventListener | null = null;
	let cursorChangedListener: google.maps.MapsEventListener | null = null;
	let animationChangedListener: google.maps.MapsEventListener | null = null;
	let dblclickListener: google.maps.MapsEventListener | null = null;
	let dragListener: google.maps.MapsEventListener | null = null;
	let dragendListener: google.maps.MapsEventListener | null = null;
	let draggableChangedListener: google.maps.MapsEventListener | null = null;
	let dragstartListener: google.maps.MapsEventListener | null = null;
	let flatChangedListener: google.maps.MapsEventListener | null = null;
	let iconChangedListener: google.maps.MapsEventListener | null = null;
	let mousedownListener: google.maps.MapsEventListener | null = null;
	let mouseoutListener: google.maps.MapsEventListener | null = null;
	let mouseoverListener: google.maps.MapsEventListener | null = null;
	let mouseupListener: google.maps.MapsEventListener | null = null;
	let positionChangedListener: google.maps.MapsEventListener | null = null;
	let rightClickListener: google.maps.MapsEventListener | null = null;
	let shapeChangedListener: google.maps.MapsEventListener | null = null;
	let titleChangedListener: google.maps.MapsEventListener | null = null;
	let visibleChangedListener: google.maps.MapsEventListener | null = null;
	let zindexChangedListener: google.maps.MapsEventListener | null = null;

	let customContainer: HTMLDivElement;
	let infoWindow: google.maps.InfoWindow | undefined;
	let marker: google.maps.marker.AdvancedMarkerElement;
	const { getMap } = getContext<{ getMap: () => google.maps.Map }>('map') ?? {};
	let map: google.maps.Map = getMap();

	onMount(async () => {
		const { AdvancedMarkerElement } = await google.maps.importLibrary('marker') as google.maps.MarkerLibrary;


		if (useInfoWindow)
			infoWindow = new google.maps.InfoWindow();

		if (map && position && customContainer) {
			customContainer.className = customClassName;

			marker = new AdvancedMarkerElement({
				position,
				map,
				...options,
				content: customContainer
			});
			onLoad?.(marker);
		}
	});

	onDestroy(() => {
		if (marker) {
			marker.map = null;
			onUnmount?.(marker);
		}
	});

	$: if (marker && onClick && browser) {
		if (clickListener !== null) {
			google.maps.event.removeListener(clickListener);
		}
		clickListener = google.maps.event.addListener(marker, 'click', onClick);
	}

	$: if (marker && onClickableChanged && browser) {
		if (clickableChangedListener !== null) {
			google.maps.event.removeListener(clickableChangedListener);
		}
		clickableChangedListener = google.maps.event.addListener(
			marker,
			'clickable_changed',
			onClickableChanged
		);
	}

	$: if (marker && onCursorChanged && browser) {
		if (cursorChangedListener !== null) {
			google.maps.event.removeListener(cursorChangedListener);
		}
		cursorChangedListener = google.maps.event.addListener(
			marker,
			'cursor_changed',
			onCursorChanged
		);
	}

	$: if (marker && onAnimationChanged && browser) {
		if (animationChangedListener !== null) {
			google.maps.event.removeListener(animationChangedListener);
		}
		animationChangedListener = google.maps.event.addListener(
			marker,
			'animation_changed',
			onAnimationChanged
		);
	}

	$: if (marker && onDoubleClick && browser) {
		if (dblclickListener !== null) {
			google.maps.event.removeListener(dblclickListener);
		}
		dblclickListener = google.maps.event.addListener(marker, 'dblclick', onDoubleClick);
	}

	$: if (marker && onDrag && browser) {
		if (dragListener !== null) {
			google.maps.event.removeListener(dragListener);
		}
		dragListener = google.maps.event.addListener(marker, 'drag', onDrag);
	}

	$: if (marker && onDragEnd && browser) {
		if (dragendListener !== null) {
			google.maps.event.removeListener(dragendListener);
		}
		dragendListener = google.maps.event.addListener(marker, 'dragend', onDragEnd);
	}

	$: if (marker && onDraggableChanged && browser) {
		if (draggableChangedListener !== null) {
			google.maps.event.removeListener(draggableChangedListener);
		}
		draggableChangedListener = google.maps.event.addListener(
			marker,
			'draggable_changed',
			onDraggableChanged
		);
	}

	$: if (marker && onDragStart && browser) {
		if (dragstartListener !== null) {
			google.maps.event.removeListener(dragstartListener);
		}
		dragstartListener = google.maps.event.addListener(marker, 'dragstart', onDragStart);
	}

	$: if (marker && onFlatChanged && browser) {
		if (flatChangedListener !== null) {
			google.maps.event.removeListener(flatChangedListener);
		}
		flatChangedListener = google.maps.event.addListener(marker, 'flat_changed', onFlatChanged);
	}

	$: if (marker && onIconChanged && browser) {
		if (iconChangedListener !== null) {
			google.maps.event.removeListener(iconChangedListener);
		}
		iconChangedListener = google.maps.event.addListener(marker, 'icon_changed', onIconChanged);
	}

	$: if (marker && onMouseDown && browser) {
		if (mousedownListener !== null) {
			google.maps.event.removeListener(mousedownListener);
		}
		mousedownListener = google.maps.event.addListener(marker, 'mousedown', onMouseDown);
	}

	$: if (marker && onMouseOut && browser) {
		if (mouseoutListener !== null) {
			google.maps.event.removeListener(mouseoutListener);
		}
		mouseoutListener = google.maps.event.addListener(marker, 'mouseout', onMouseOut);
	}

	$: if (marker && onMouseOver && browser) {
		if (mouseoverListener !== null) {
			google.maps.event.removeListener(mouseoverListener);
		}
		mouseoverListener = google.maps.event.addListener(marker, 'mouseover', onMouseOver);
	}

	$: if (marker && onMouseUp && browser) {
		if (mouseupListener !== null) {
			google.maps.event.removeListener(mouseupListener);
		}
		mouseupListener = google.maps.event.addListener(marker, 'mouseup', onMouseUp);
	}

	$: if (marker && onPositionChanged && browser) {
		if (positionChangedListener !== null) {
			google.maps.event.removeListener(positionChangedListener);
		}
		positionChangedListener = google.maps.event.addListener(
			marker,
			'position_changed',
			onPositionChanged
		);
	}

	$: if (marker && onRightClick && browser) {
		if (rightClickListener !== null) {
			google.maps.event.removeListener(rightClickListener);
		}
		rightClickListener = google.maps.event.addListener(marker, 'rightclick', onRightClick);
	}

	$: if (marker && onShapeChanged && browser) {
		if (shapeChangedListener !== null) {
			google.maps.event.removeListener(shapeChangedListener);
		}
		shapeChangedListener = google.maps.event.addListener(marker, 'shape_changed', onShapeChanged);
	}

	$: if (marker && onTitleChanged && browser) {
		if (titleChangedListener !== null) {
			google.maps.event.removeListener(titleChangedListener);
		}
		titleChangedListener = google.maps.event.addListener(marker, 'title_changed', onTitleChanged);
	}

	$: if (marker && onVisibleChanged && browser) {
		if (visibleChangedListener !== null) {
			google.maps.event.removeListener(visibleChangedListener);
		}
		visibleChangedListener = google.maps.event.addListener(
			marker,
			'visible_changed',
			onVisibleChanged
		);
	}

	$: if (marker && onZindexChanged && browser) {
		if (zindexChangedListener !== null) {
			google.maps.event.removeListener(zindexChangedListener);
		}
		zindexChangedListener = google.maps.event.addListener(
			marker,
			'zindex_changed',
			onZindexChanged
		);
	}

	$: if (useInfoWindow && !infoWindow && browser) {
		infoWindow = new google.maps.InfoWindow();
	}

	$: if (useInfoWindow && infoWindow && infoWindowContent !== infoWindow.getContent() && browser)
		infoWindow.setContent(infoWindowContent);
</script>

<div bind:this={customContainer}>
	<slot />
</div>
