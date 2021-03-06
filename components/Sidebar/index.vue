<template>
  <div class="sidebar">
    <div class="group" v-for="sidebarGroupItem, index in sidebars" v-if="sidebarGroupItem">
      <div class="group__title">{{ sidebarGroupOrder[index] }}</div>
      <div class="group__body">

        <!-- render README.md in this folder -->
        <div v-if="sidebarGroupItem.to" :class="[
          'group__category',
          'category',
          {
            'category--selected': $route.fullPath === sidebarGroupItem.to,
            'category--active': $route.fullPath === sidebarGroupItem.to,
          }
        ]">
          <div class="category__label">
            <SidebarLink :to="sidebarGroupItem.to">{{ title(sidebarGroupItem.title || sidebarGroupOrder[index]) }}</SidebarLink>
          </div>
        </div>

        <!-- render headers of README.md in this folder -->
        <div v-if="sidebarGroupItem.headers && sidebarGroupItem.headers.length" v-for="header in sidebarGroupItem.headers" :class="[
          'group__category',
          'category',
          {
            'category--selected': $route.fullPath === `${sidebarGroupItem.to}#${header.slug}`,
            'category--active': $route.fullPath === `${sidebarGroupItem.to}#${header.slug}`,
          }
        ]">
          <div class="category__label">
            <SidebarLink :to="`${sidebarGroupItem.to}#${header.slug}`">{{ title(header.title) }}</SidebarLink>
          </div>
        </div>

        <!-- render other files in this folder -->
        <div v-if="sidebarGroupItem.children && sidebarGroupItem.children.length" v-for="child in sidebarGroupItem.children" :name="`${child.to}`" :class="[
          'group__category',
          'category',
          {
            'category--selected': !child.isLangNav && $route.path === child.to,
            'category--active': !child.isLangNav && $route.fullPath === child.to,
          }
        ]">
          <div class="category__label">
            <SidebarLink :to="child.to">{{ title(child.title) }}</SidebarLink>
          </div>
          <div v-if="child.headers && child.headers.length" v-for="header in child.headers" :class="[
            'category__headers',
            {
              'category--active': $route.fullPath === `${child.to}#${header.slug}`,
            }
          ]">
            <div class="category__header-item">
              <SidebarLink :to="`${child.to}#${header.slug}`">{{ title(header.title) }}</SidebarLink>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import config from '../../config'
import { title } from '../../utils'
import SidebarLink from '../SidebarLink'

export default {
  name: 'Sidebar',
  components: {
    SidebarLink,
  },
  props: {
    items: {
      type: Object,
      required: true,
    },
  },
  computed: {
    sidebarGroupOrder() {
      const groupOrderConfig = config.get(
        this.$site,
        'sidebarGroupOrder',
        this.$localePath
      )

      if (groupOrderConfig) {
        const result = groupOrderConfig.slice()

        return result
      } else {
        return Object.keys(this.items)
      }
    },
    sidebars() {
      return this.sidebarGroupOrder
        .map(item => {
          return this.items[item]
        })
    },
  },
  methods: {
    title,
  },
}
</script>

<style lang="stylus">
@import '../../styles/_variables.styl'

.sidebar
  position: fixed
  top: 3.6rem
  bottom: 0
  width: 100%
  overflow: auto
  background: $white

.group
  margin-bottom: 4rem
  &:first-child
    margin-top: 20px

  &__title
    padding-left: 30px
    margin-bottom: 1em
    font-size: 14px
    font-weight: 300
    letter-spacing: 1.3px
    text-transform: uppercase
    color: #888

.category
  a
    color: $black

  a:hover
    color: $sidebar-active

  a.router-link-exact-active.router-link-active
    color: $sidebar-active

  &__label,
  &__header-item
    height: 2em
    margin: 0.6em 0
    line-height: 2em

  &__label,
  &__headers
    border-left: 4px solid $white

  &__label
    padding-left: 26px

  &__headers
    display: none

  &--active,
  &--selected
    & ^[0]__headers
      display: block

  &--selected &__label a
    color: $sidebar-active

  &--active &__label,
  &--active&__headers
      font-weight: 600
      border-color: $sidebar-active

  &__header-item
    padding-left: 30px

    &::before
      margin-right: 4px
      color: #979797
      content: "-"
</style>
