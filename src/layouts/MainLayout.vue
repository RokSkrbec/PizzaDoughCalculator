<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>

        <q-toolbar-title>
          Pizza dough Calculator
        </q-toolbar-title>
        <q-btn flat round @click="$q.dark.toggle()"
          :icon="$q.dark.isActive ? 'brightness_2' : 'brightness_5'" />
      </q-toolbar>
    </q-header>

    <!--q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header>
          Essential Links
        </q-item-label>

        <EssentialLink v-for="link in essentialLinks" :key="link.title"
          v-bind="link" />
      </q-list>
    </q-drawer-->

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import EssentialLink, { EssentialLinkProps } from 'components/EssentialLink.vue';

const essentialLinks: EssentialLinkProps[] = [
  {
    title: 'Home',
    caption: 'Home Page',
    icon: 'school',
    link: '/'
  },
  {
    title: 'Calculator',
    caption: 'Calculate your dough hydration',
    icon: 'school',
    link: '/calculator'
  },
];

const leftDrawerOpen = ref(false)
const mode = ref(true)

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>
