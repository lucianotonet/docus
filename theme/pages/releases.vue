<template>
  <AppPage>
    <div class="mb-10">
      <h1 class="flex items-center justify-between text-4xl font-bold tracking-tight text-gray-900 dark:text-gray-100">
        <span class="flex-1">Releases</span>
      </h1>
    </div>
    <div class="prose max-w-none dark:prose-dark nuxt-content">
      <div v-for="release of releases" :key="release.name">
        <h2 :id="release.name" class="flex items-center justify-between">
          <a :href="`#${release.name}`">
            {{ release.name }}
          </a>
          <span
            class="text-base font-normal text-gray-500"
          >{{ formatDate(release) }}</span>
        </h2>

        <NuxtContent :document="release.body" />
      </div>
    </div>
    <hr class="mt-10 mb-4 border-gray-200 dark:border-gray-800">
    <template #toc>
      <PageToc :toc="toc" />
    </template>
  </AppPage>
</template>

<script>
export default {
  layout ({ $docus }) {
    return $docus.settings.layout
  },
  async asyncData ({ $docus }) {
    const releases = await $docus.fetchReleases()

    return { releases }
  },
  head () {
    return {
      title: 'Releases'
    }
  },
  computed: {
    settings () {
      return this.$docus.settings
    },
    toc () {
      return this.releases.map(release => ({ id: release.name, depth: 2, text: release.name }))
    }
  },
  methods: {
    formatDate (release) {
      const date = new Date(release.date)

      return date.toLocaleDateString(this.$i18n.locale)
    }
  }
}
</script>
