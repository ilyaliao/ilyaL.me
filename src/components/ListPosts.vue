<script setup lang="ts">
import type { Post } from '~/types'
import { useRouter } from 'vue-router/auto'
import { formatDate } from '~/logics'

const { type, posts, extra } = defineProps<{
  type?: string
  posts?: Post[]
  extra?: Post[]
}>()

const router = useRouter()
const routes: Post[] = router.getRoutes()
  .filter(i => i.path.startsWith('/posts') && i.meta.frontmatter.date && !i.meta.frontmatter.draft)
  .filter(i => !i.path.endsWith('.html') && (i.meta.frontmatter.type || 'blog').split('+').includes(type))
  .map(i => ({
    path: i.meta.frontmatter.redirect || i.path,
    title: i.meta.frontmatter.title,
    date: i.meta.frontmatter.date,
    duration: i.meta.frontmatter.duration,
    redirect: i.meta.frontmatter.redirect,
    place: i.meta.frontmatter.place,
  }))

const _posts = computed(() =>
  [...(posts || routes), ...extra || []]
    .sort((a, b) => +new Date(b.date) - +new Date(a.date)),
)
</script>

<template>
  <ul>
    <template v-if="!_posts.length">
      <div py-2 op-50>
        { nothing here yet }
      </div>
    </template>

    <template v-for="route, idx in _posts" :key="route.path">
      <div
        class="slide-enter"
        :style="{
          '--enter-stage': idx,
          '--enter-step': '60ms',
        }"
      >
        <component
          :is="route.path.includes('://') ? 'a' : 'RouterLink'"
          v-bind="
            route.path.includes('://') ? {
              href: route.path,
              target: '_blank',
              rel: 'noopener noreferrer',
            } : {
              to: route.path,
            }
          "
          class="item block font-normal mb-6 mt-2 no-underline"
        >
          <li no-underline flex="~ col md:row gap-2 md:items-center">
            <div text-lg lh-1.2em flex="~ gap-2 wrap" class="title">
              <span align-middle>{{ route.title }}</span>
              <span
                v-if="route.redirect"
                align-middle op-50 flex-none text-xs ml--1.5
                i-carbon-arrow-up-right
                title="External"
              />
            </div>

            <div flex="~ gap-2 items-center">
              <span text-sm op-50 ws-nowrap>
                {{ formatDate(route.date, true) }}
              </span>
              <span v-if="route.duration" text-sm op-40 ws-nowrap>· {{ route.duration }}</span>
              <span v-if="route.platform" text-sm op-40 ws-nowrap>· {{ route.platform }}</span>
              <span v-if="route.place" text-sm op-40 ws-nowrap md:hidden>· {{ route.place }}</span>
            </div>
          </li>
          <div v-if="route.place" op-50 text-sm hidden mt--2 md:block>
            {{ route.place }}
          </div>
        </component>
      </div>
    </template>
  </ul>
</template>
