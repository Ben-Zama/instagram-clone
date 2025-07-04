<template>
  <q-layout view="hHh lpr fFf" class="main-layout">

    <!-- Header -->
    <q-header class="q-pt-xs q-pb-xs">
      <q-toolbar>
        <q-img src="src/assets/logo.png" />
        <q-toolbar-title class="text-h5">Instagram</q-toolbar-title>
        <q-btn round dense flat size="10px" padding="10px" icon="bi-bell" @click="notificationSlider = true" />
      </q-toolbar>
    </q-header>

    <!-- Drawer overlays -->
    <q-drawer v-model="notificationSlider" side="right" bordered>
      <div class="q-pa-md row justify-between items-center shadow-1">
        <div class="text-center text-h6">Notifications</div>
        <q-btn dense round flat color="red" icon="close" @click="closeNotificationSlider" />
      </div>
      <q-scroll-area class="fit">
        <q-item v-for="item in notifications" :key="item.id" clickable v-ripple>
          <q-item-section top avatar>
            <q-avatar color="primary" text-color="white">
              <q-icon :name="item.icon || 'bi-bell'" />
            </q-avatar>
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ item.title }}</q-item-label>
            <q-item-label caption lines="2">
              {{ item.message }}
            </q-item-label>
          </q-item-section>
          <q-item-section side>
            <q-item-label caption>
              {{ formatTime(item.timestamp) }}
            </q-item-label>
          </q-item-section>
        </q-item>
      </q-scroll-area>
    </q-drawer>

    <!-- Page -->
    <q-page-container>
      <router-view />
    </q-page-container>

    <!-- Footer -->
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
import { ref, onMounted } from 'vue'
import axios from 'axios'

const notificationSlider = ref(false)
const notifications = ref([])

// Close drawer
const closeNotificationSlider = () => {
  notificationSlider.value = false
}

// Fetch notifications
const fetchNotifications = async () => {
  try {
    const { data } = await axios.get(
      'https://fake-json-api.mock.beeceptor.com/notifications'
    )
    notifications.value = data.map(item => ({
      ...item,
      icon: getIcon(item.type)
    }))
  } catch (err) {
    console.error('Error fetching notifications:', err)
  }
}

onMounted(fetchNotifications)

// Format timestamp (ISO 8601) - return only day and month
function formatTime(timestamp) {
  if (!timestamp) return ''
  const date = new Date(timestamp)
  return date.toLocaleDateString(undefined, { day: 'numeric', month: 'short' })
}

// Map notification types to icons
function getIcon(type) {
  switch (type) {
    case 'announcement':
      return 'bi-megaphone-fill'
    case 'promotion':
      return 'bi-bell-fill'
    case 'order':
      return 'bi-cart-fill'
    case 'service':
      return 'bi-people-fill'
    case 'review':
      return 'bi-star-fill'
    default:
      return 'bi-bell'
  }
}
</script>
