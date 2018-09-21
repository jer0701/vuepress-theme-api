<template>
  <div 
    class="theme__container"
    :class="pageClasses"
    @touchstart="onTouchStart"
    @touchend="onTouchEnd">
    <Navbar
      v-if="shouldShowNavbar"
      @toggle-sidebar="toggleSidebar"
    />
    <div
      class="sidebar-mask"
      @click="toggleSidebar(false)"
    ></div>
    <div class="row" v-if="shouldShowSidebar">
      <div class="col-md-2">
        <Sidebar 
            :items="sidebarItems"
            @toggle-sidebar="toggleSidebar">
          <slot name="sidebar-top" slot="top" />
          <slot name="sidebar-bottom" slot="bottom" />
        </Sidebar>
      </div>
      <div class="col-md-10">
        <div class="custom__layout" v-if="$page.frontmatter.layout">
          <component :is="$page.frontmatter.layout"/>
        </div>
        <Home v-else-if="$page.frontmatter.home"/>
        <Page v-else></Page>
      </div>
    </div>
    <div v-else>
      <div class="custom__layout" v-if="$page.frontmatter.layout">
        <component :is="$page.frontmatter.layout"/>
      </div>
      <Home v-else-if="$page.frontmatter.home"/>
      <Page v-else></Page>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import nprogress from 'nprogress'
import Navbar from './components/Navbar'
import Sidebar from './components/Sidebar'
import Home from './Home'
import Page from './Page'

import { resolveSidebarItems } from './utils'

export default {
  components: {
    Home,
    Navbar,
    Sidebar,
    Page,
  },
  data() {
    return {
      isSidebarOpen: true,
    }
  },
  computed: {
    shouldShowNavbar () {
      const { themeConfig } = this.$site
      const { frontmatter } = this.$page
      if (
        frontmatter.navbar === false ||
        themeConfig.navbar === false) {
        return false
      }
      return (
        this.$title ||
        themeConfig.logo ||
        themeConfig.repo ||
        themeConfig.nav ||
        this.$themeLocaleConfig.nav
      )
    },
    sidebarItems() {
      return resolveSidebarItems(this.$page, this.$site, this.$localePath)
    },
    shouldShowSidebar() {
      const { frontmatter } = this.$page

      return (
        !frontmatter.home &&
        frontmatter.sidebar !== false &&
        Object.keys(this.sidebarItems).length
      )
    },
    pageClasses() {
      const userPageClass = this.$page.frontmatter.pageClass

      return [
        {
          'sidebar-open': this.shouldShowSidebar && this.isSidebarOpen,
          'no-sidebar': !this.shouldShowSidebar,
        },
        userPageClass,
      ]
    },
  },
  mounted() {
    // configure progress bar
    nprogress.configure({ showSpinner: false })

    this.$router.beforeEach((to, from, next) => {
      if (to.path !== from.path && !Vue.component(to.name)) {
        nprogress.start()
      }

      next()
    })

    this.$router.afterEach(() => {
      nprogress.done()
    })
  },
  created() {
    if (this.$ssrContext) {
      this.$ssrContext.title = this.$title
      this.$ssrContext.lang = this.$lang
      this.$ssrContext.description = this.$page.description || this.$description
    }
  },
  methods: {
    toggleSidebar (to) {
      this.isSidebarOpen = typeof to === 'boolean' ? to : !this.isSidebarOpen
    },
    // side swipe
    onTouchStart (e) {
      this.touchStart = {
        x: e.changedTouches[0].clientX,
        y: e.changedTouches[0].clientY
      }
    },
    onTouchEnd (e) {
      const dx = e.changedTouches[0].clientX - this.touchStart.x
      const dy = e.changedTouches[0].clientY - this.touchStart.y
      if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 40) {
        if (dx > 0 && this.touchStart.x <= 80) {
          this.toggleSidebar(true)
        } else {
          this.toggleSidebar(false)
        }
      }
    }
  }
}
</script>

<style src="prismjs/themes/prism-tomorrow.css">
</style>
<style src="./styles/theme.styl" lang="stylus"></style>
