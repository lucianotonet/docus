<template>
  <div class="w-full">
    <slot v-if="header" name="header"><AppHeader :aside="aside" /></slot>
    <div class="lg:flex" :class="fluid ? '' : 'mx-auto max-w-8xl'">
      <slot v-if="aside" name="aside"><AppAside /></slot>
      <div class="flex-auto w-full min-w-0 lg:static lg:max-h-full lg:overflow-visible">
        <slot />
      </div>
    </div>
    <slot name="footer" />
    <DMadeWithDocus v-if="$docus.settings.credits" />
  </div>
</template>

<script>
export default {
  props: {
    fluid: {
      type: Boolean,
      default: false
    },
    header: {
      type: Boolean,
      default: true
    },
    aside: {
      type: Boolean,
      default: false
    }
  }
}
</script>
