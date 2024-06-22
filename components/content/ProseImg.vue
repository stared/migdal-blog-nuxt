<template>
  <figure>
    <component
      :is="imgComponent"
      :src="refinedSrc"
      :alt="alt"
      :width="width"
      :height="height"
    />
    <figcaption>{{ alt }}</figcaption>
  </figure>
</template>

<script setup lang="ts">
// approach from https://github.com/nuxt/image/issues/813

import { withTrailingSlash, withLeadingSlash, joinURL } from "ufo";
import { useRuntimeConfig, computed, resolveComponent } from "#imports";

const imgComponent = useRuntimeConfig().public.mdc.useNuxtImage
  ? resolveComponent("NuxtImg")
  : "img";

const props = defineProps({
  src: {
    type: String,
    default: "",
  },
  alt: {
    type: String,
    default: "",
  },
  width: {
    type: [String, Number],
    default: undefined,
  },
  height: {
    type: [String, Number],
    default: undefined,
  },
});

const refinedSrc = computed(() => {
  if (props.src?.startsWith("/") && !props.src.startsWith("//")) {
    const _base = withLeadingSlash(
      withTrailingSlash(useRuntimeConfig().app.baseURL)
    );
    if (_base !== "/" && !props.src.startsWith(_base)) {
      return joinURL(_base, props.src);
    }
  }
  return props.src;
});
</script>