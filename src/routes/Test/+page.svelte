<script>
    import maplibregl from 'maplibre-gl' // Ensure maplibre is imported
    import {
        Control,
        ControlButton,
        ControlGroup,
        MapLibre
    } from 'svelte-maplibre' // Import only what you're using

    let map // Declare the map variable

    // Define markers
    const markers = [
        {
            lngLat: { lng: 144.98, lat: -37.805 },
            label: 'Marker 1',
            name: 'This is a marker',
        },
        {
            lngLat: { lng: 144.98, lat: -37.81 },
            label: 'Marker 2',
            name: 'This is a marker',
        },
        {
            lngLat: { lng: 144.96, lat: -37.81 },
            label: 'Marker 3',
            name: 'This is a marker',
        },
        {
            lngLat: { lng: 144.96, lat: -37.82 },
            label: 'Marker 4',
            name: 'This is a new marker',
        }
    ]

    function getMapBounds(markers) {
        const bounds = new maplibregl.LngLatBounds()
        markers.forEach((marker) => {
            bounds.extend([marker.lngLat.lng, marker.lngLat.lat])
        })
        return bounds
    }

    let bounds = getMapBounds(markers) // Compute bounds from markers

    $: {
        if (bounds && map) {
            map.fitBounds(bounds)
        }
    }
</script>

<!-- MapLibre component with map functionality -->
<MapLibre
    bind:this={map}
    center={[144.97, -37.81]}
    class="map flex-grow min-h-[500px]"
    standardControls
    style="https://tiles.basemaps.cartocdn.com/gl/voyager-gl-style/style.json"
    zoom={14}
>
    <!-- Adding control buttons -->
    <Control class="flex flex-col gap-y-2">
        <ControlGroup>
            <ControlButton on:click={() => { bounds = getMapBounds(markers) }}>
                Fit to bounds
            </ControlButton>
        </ControlGroup>
    </Control>
</MapLibre>
