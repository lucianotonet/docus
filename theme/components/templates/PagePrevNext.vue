<template>
  <div v-if="prev || next" class="flex flex-col justify-between py-4 font-medium leading-7 xs:space-x-2 xs:flex-row">
    <div class="flex justify-start w-full xs:w-1/2">
      <NuxtLink
        v-if="prev"
        :to="$contentLocalePath(prev.to)"
        class="inline-flex items-center justify-end w-full px-4 py-4 truncate border border-gray-100 rounded group min-w-1/2 xs:w-auto xs:justify-end dark:text-gray-400 dark:border-gray-800 dark:hover:text-primary-400 hover:text-primary-500 hover:border-primary-400 dark:hover:border-gray-700"
      >
        <span class="relative flex flex-col items-end truncate">
          <span v-if="prev.category" class="mb-1 text-xs leading-tight text-gray-400 group-hover:text-primary-400">{{ prev.category }}</span>
          <span class="flex items-center w-full leading-tight">
            <IconArrowLeft class="flex-shrink-0 w-4 h-4 mr-2 text-primary-500" />
            <span class="truncate text-primary-500">{{ prev.title }}</span>
          </span>
        </span>
      </NuxtLink>
    </div>

    <div class="flex justify-end order-first w-full xs:order-last xs:w-1/2">
      <NuxtLink
        v-if="next"
        :to="$contentLocalePath(next.to)"
        class="inline-flex items-center justify-start w-full px-4 py-4 mb-2 truncate border border-gray-100 rounded group xs:mb-0 xs:w-auto min-w-1/2 xs:justify-start dark:text-gray-400 dark:border-gray-800 dark:hover:text-primary-400 hover:text-primary-500 hover:border-primary-400 dark:hover:border-gray-700"
      >
        <span class="relative flex flex-col items-start truncate">
          <span v-if="next.category" class="mb-1 text-xs leading-tight text-gray-400 group-hover:text-primary-400">{{ next.category }}</span>
          <span class="flex items-center w-full leading-tight">
            <span class="truncate text-primary-500">{{ next.title }}</span>
            <IconArrowRight class="flex-shrink-0 w-4 h-4 ml-2 text-primary-500" />
          </span>
        </span>
      </NuxtLink>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    prev: {
      type: Object,
      default: () => null
    },
    next: {
      type: Object,
      default: () => null
    }
  }
}
</script>
