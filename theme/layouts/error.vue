<template>
  <AppContainer aside>
    <AppPage prose>
      <h1>{{ error.message }}</h1>
      <p v-if="error.statusCode === 404">Go back <NuxtLink to="/">home</NuxtLink>.</p>
    </AppPage>
  </AppContainer>
</template>

<script>
export default {
  props: {
    error: {
      type: Object,
      required: true
    }
  },
  head () {
    return {
      title: this.error.message
    }
  }
}
</script>
