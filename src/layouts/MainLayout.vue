<template>
  <q-layout class="main-layout">
    <q-header class="q-pt-xs q-pb-xs" bordered>
      <q-toolbar>
        <q-img src="src/assets/logo.png" />
        <q-toolbar-title class="text-h5">Instagram</q-toolbar-title>
        <q-btn round dense flat size="10px" padding="10px" icon="bi-bell" @click="notifications = true" />
      </q-toolbar>
    </q-header>

    <q-drawer v-model="notifications" side="right" bordered> </q-drawer>

    <q-page-container>
      <q-page class="q-pa-md">
        <router-view />
      </q-page>
    </q-page-container>

    <q-footer bordered>
      <q-toolbar class="justify-evenly">
        <router-link to="/" v-slot="{ navigate, href, isActive }">
          <q-btn size="10px" padding="10px" :to="href" @click="navigate" round dense flat
            :icon="isActive ? 'bi-house-door-fill' : 'bi-house-door'" />
        </router-link>

        <router-link to="/explore" v-slot="{ navigate, href, isActive }">
          <q-btn size="10px" padding="10px" :to="href" @click="navigate" round dense flat
            :icon="isActive ? 'bi-compass-fill' : 'bi-compass'" />
        </router-link>

        <router-link to="/reels" v-slot="{ navigate, href, isActive }">
          <q-btn size="10px" padding="10px" :to="href" @click="navigate" round dense flat
            :icon="isActive ? 'bi-file-play-fill' : 'bi-file-play'" />
        </router-link>

        <router-link to="/chat" v-slot="{ navigate, href, isActive }">
          <q-btn size="10px" padding="10px" :to="href" @click="navigate" round dense flat
            :icon="isActive ? 'bi-chat-dots-fill' : 'bi-chat-dots'" />
        </router-link>

        <router-link to="/profile">
          <q-avatar size="24px">
            <q-img src="/src/assets/headshot.jpg" />
          </q-avatar>
        </router-link>
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script setup>
import { ref } from "vue";

const notifications = ref(false);
</script>
