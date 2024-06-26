<script lang="ts">
	import { getContext, onDestroy, onMount } from 'svelte';
	import { BROWSER as browser } from 'esm-env';

	export let position: google.maps.LatLng | google.maps.LatLngLiteral;
	export let options: google.maps.marker.AdvancedMarkerElementOptions;

	export let onClick: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onClickableChanged: () => void = () => {
	};
	export let onCursorChanged: () => void = () => {
	};
	export let onAnimationChanged: () => void = () => {
	};
	export let onDblClick: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onDrag: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onDragEnd: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onDraggableChanged: () => void = () => {
	};
	export let onDragStart: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onFlatChanged: () => void = () => {
	};
	export let onIconChanged: () => void = () => {
	};
	export let onMouseDown: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onMouseOut: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onMouseOver: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onMouseUp: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onPositionChanged: () => void = () => {
	};
	export let onRightClick: (e: google.maps.MapMouseEvent) => void = () => {
	};
	export let onShapeChanged: () => void = () => {
	};
	export let onTitleChanged: () => void = () => {
	};
	export let onVisibleChanged: () => void = () => {
	};
	export let onZindexChanged: () => void = () => {
	};
	export let onLoad: (marker: google.maps.marker.AdvancedMarkerElement) => void = () => {
	};
	export let onUnmount: (marker: google.maps.marker.AdvancedMarkerElement) => void = () => {
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
	let rightclickListener: google.maps.MapsEventListener | null = null;
	let shapeChangedListener: google.maps.MapsEventListener | null = null;
	let titleChangedListener: google.maps.MapsEventListener | null = null;
	let visibleChangedListener: google.maps.MapsEventListener | null = null;
	let zindexChangedListener: google.maps.MapsEventListener | null = null;

	let marker: google.maps.marker.AdvancedMarkerElement;
	const { getMap } = getContext<{ getMap: () => google.maps.Map }>('map') ?? {};
	let map = getMap();



	onMount(async () => {
		const { AdvancedMarkerElement } = await google.maps.importLibrary("marker") as google.maps.MarkerLibrary;

		if (map && position) {
			marker = new AdvancedMarkerElement({
				position,
				map: getMap(),
				...options
			});
			onLoad?.(marker);
		}
	})

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

	$: if (marker && onDblClick && browser) {
		if (dblclickListener !== null) {
			google.maps.event.removeListener(dblclickListener);
		}
		dblclickListener = google.maps.event.addListener(marker, 'dblclick', onDblClick);
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
		if (rightclickListener !== null) {
			google.maps.event.removeListener(rightclickListener);
		}
		rightclickListener = google.maps.event.addListener(marker, 'rightclick', onRightClick);
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
</script>

<slot />
