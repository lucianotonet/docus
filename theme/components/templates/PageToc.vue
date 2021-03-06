<template>
  <div v-if="toc.length" class="flex-none hidden w-64 pl-8 mr-8 xl:text-sm xl:block ">
    <PageTocTop />
    <div class="flex flex-col justify-between overflow-y-auto sticky max-h-(screen-18) -mt-10 pt-10 pb-4 top-18">
      <h5 class="flex items-center mb-1">
        <span class="text-sm font-semibold text-gray-900 dark:text-gray-100 ">{{ $t('toc.title') }}</span>
      </h5>

      <ul class="overflow-x-hidden font-medium">
        <li
          v-for="link of toc"
          :key="link.id"
          class="hover:text-gray-900 dark:hover:text-gray-100 "
          :class="{
            'text-primary-500 hover:text-primary-500 dark:text-primary-400 dark:hover:text-primary-400': exactActiveLink === link.id || activeLink === link.id,
            'text-gray-700 dark:text-gray-200': !(exactActiveLink === link.id || activeLink === link.id)
          }"
        >
          <a
            :href="`#${link.id}`"
            class="block py-1 transition-colors duration-100 transform scrollactive-item "
            :class="{
              '': link.depth === 2,
              'border-l-2 border-gray-100 dark:border-gray-800 pl-3 text-gray-500 dark:text-gray-400': link.depth === 3,
              'dark:border-primary-500 border-primary-500 text-primary-400': link.depth === 3 && (exactActiveLink === link.id || activeLink === link.id)
            }"
          >{{ link.text }}</a>
        </li>
      </ul>
      <PageTocBottom />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    toc: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      activeLink: '',
      exactActiveLink: '',
      sections: []
    }
  },
  computed: {
    settings () {
      return this.$docus.settings
    }
  },
  beforeMount () {
    history.scrollRestoration = 'manual'
  },
  mounted () {
    document
      .querySelectorAll('.nuxt-content h2[id], .nuxt-content h3[id]')
      .forEach((section) => {
        this.sections.push({
          level: section.tagName.replace(/h/i, ''),
          id: section.getAttribute('id'),
          top: section.offsetTop
        })
      })
    const hash = window.location.hash.replace('#', '')
    const hashIndex = this.sections.findIndex(section => section.id === hash)
    if (hash && hashIndex >= 0) {
      const offset = document.querySelector(location.hash).offsetTop - 110 // 110 is the deafult value for `top-margin-scroll` in tailwind prose
      this.$nextTick().then(() => {
        scrollTo(0, offset)
        this.setActive(hashIndex)
      })
    } else {
      this.onScroll()
    }
    window.addEventListener('scroll', this.onScroll)
  },
  beforeDestroy () {
    window.removeEventListener('scroll', this.onScroll, true)
  },
  methods: {
    onScroll () {
      const yOffset = window.pageYOffset
      const windowHeight = window.innerHeight
      if (yOffset === 0) {
        this.setActive(0)
      } else if (yOffset + windowHeight >= document.body.scrollHeight) {
        return this.setActive(this.sections.length - 1)
      } else {
        const targetPoint = yOffset + windowHeight / 2
        let index = 0
        for (let i = 0; i < this.sections.length; i++) {
          if (this.sections[i].top <= targetPoint) {
            index = i
          }
        }
        this.setActive(index)
      }
    },
    setActive (index) {
      if (!this.sections[index]) {
        return
      }
      this.exactActiveLink = this.sections[index].id
      this.activeLink = this.sections[index].id
      if (this.sections[index].level === '3') {
        let parentIndex = -1
        for (let i = 0; i < index; i++) {
          if (this.sections[i].level === '2') {
            parentIndex = i
          }
        }
        if (parentIndex >= 0) {
          this.activeLink = this.sections[parentIndex].id
        }
      }
    }
  }
}
</script>
