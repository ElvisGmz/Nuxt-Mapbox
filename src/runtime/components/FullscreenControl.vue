<script setup lang="ts">
import { FullscreenControl, FullscreenControlOptions } from 'mapbox-gl'
import { onUnmounted, useMapbox, onMounted, inject, ref } from "#imports";

interface Props {
    options?: FullscreenControlOptions;
    position?: "top-left" | "top-right" | "bottom-left" | "bottom-right";
}
const props = withDefaults(defineProps<Props>(), {
    options: () => ({}),
    position: () => "top-right",
});

const mapId = inject<string>("MapID");
if (!mapId) throw "Mapbox Controls must be placed inside a Map component";

const controlRef = ref<FullscreenControl>();

onMounted(() => {
    useMapbox(mapId, (map) => {
        //@ts-ignore
        const control = new FullscreenControl(props.options);
        controlRef.value = control;

        map.addControl(control, props.position);
    });
});

onUnmounted(() => {
    useMapbox(mapId, (map) => {
        if (controlRef.value) map.removeControl(controlRef.value);
    })
});
</script>

<template>
  <div>
    <slot />
  </div>
</template>
