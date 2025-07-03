<template>
  <q-layout view="hHh lpr fFf" class="main-layout">
    <q-header class="q-pt-xs q-pb-xs">
      <q-toolbar>
        <q-img src="src/assets/logo.png" />
        <q-toolbar-title class="text-h5">Davegram</q-toolbar-title>
        <q-btn round dense flat size="10px" padding="10px" icon="bi-bell" @click="notifications = true" />
      </q-toolbar>
    </q-header>

    <q-drawer v-model="notifications" side="right" bordered>
      <div class="bg-transparent q-pa-md row justify-between items-center shadow-1">
        <div class="text-center text-h6">Notifications</div>
        <q-btn dense round flat color="red" icon="close" @click="closeNotifications" />
      </div>
      <q-scroll-area class="fit">
        <q-item v-for="n in 50" :key="n" clickable v-ripple>
          <q-item-section top avatar>
            <q-avatar color="primary" text-color="white" icon="bi-bell" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Single line item</q-item-label>
            <q-item-label caption lines="2">Secondary line text.</q-item-label>
          </q-item-section>
          <q-item-section side>
            <q-item-label caption>5 min ago</q-item-label>
          </q-item-section>
        </q-item>
      </q-scroll-area>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer>
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

const closeNotifications = () => {
  notifications.value = false;
}
</script>
