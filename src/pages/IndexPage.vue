<template>
  <q-page class="feed">

    <!-- Story section -->
    <q-scroll-area class="story-section">
      <div style="height: 100%" class="row no-wrap items-center q-gutter-x-md">

        <!-- User story -->
        <q-avatar class="user-story-avatar" size="75px">
          <q-img class="fit" src="/src/assets/headshot.jpg" />
          <div class="add-story-btn-outline">
            <q-btn class="add-story-btn" flat rounded dense icon="bi-plus" />
          </div>
        </q-avatar>

        <!-- Other stories -->
        <div class="story-container" v-for="(user, index) in stories" :key="index">
          <div class="ring">
          <div class="inner-ring">
            <q-avatar size="70px">
              <q-img ratio="1" :src="user.picture.medium" />
            </q-avatar>
          </div>
        </div>
        <p class="text-caption q-ma-none">{{ user.name.first }}</p>
        </div>
      </div>
    </q-scroll-area>

    <!-- Posts section -->
    <div class="posts-section q-pa-md">

      <q-card v-for="post in posts" :key="post.id" class="post">
        <q-img ratio="1" loading="lazy" :src="`https://picsum.photos/500/300?random=${Math.random()}`">

          <!-- Post Header -->
          <div class="title-card absolute-top row items-center q-gutter-md">
            <q-avatar size="35px">
              <q-img :src="post.user.picture.medium" />
            </q-avatar>
            <div class="text">
              <p class="username">{{ post.user.name.first }}</p>
              <p class="subtitle">Original Audio</p>
            </div>
            <q-btn dense flat class="absolute-right q-pa-xs" icon="bi-three-dots" size="12.5px"></q-btn>
          </div>
        </q-img>

        <!-- Post Interactions -->
        <q-card-actions align=between>
          <div class="q-gutter-sm">
            <q-btn @click="toggleLike(post)" flat dense round :icon="post.liked ? 'bi-heart-fill' : 'bi-heart'" padding="5px" size="10px" />
            <q-btn flat dense round icon="bi-chat-right" padding="5px" size="10px" />
            <q-btn flat dense round icon="bi-send" padding="5px" size="10px" />
          </div>
          <q-btn @click="toggleSave" flat dense round :icon="saved ? 'bi-bookmark-fill' : 'bi-bookmark'" padding="5px" size="10px" />
        </q-card-actions>

        <!-- Post likes -->
        <q-card-section class="likes q-pa-none q-pl-sm avatar-container">
          <div class="avatars">
            <q-avatar v-for="(n, i) in 3" :key="n" size="17.5px" class="avatar"
              :style="{ left: `${i * 10}px`, zIndex: 3 - i }">
              <img src="/src/assets/headshot.jpg">
            </q-avatar>
          </div>
          <span class="text-weight-bold">{{ post.reactions.likes.toLocaleString() }} likes</span>
        </q-card-section>

        <!-- Post caption & comments -->
        <q-card-section class="q-pa-sm">
          <span class="text-bold">{{ post.title }}</span>
          <span class="text-caption ellipsis-2-lines">{{ post.body }}</span>
          <p class="text-primary flex q-gutter-xs q-ma-none">
            <span v-for="tag in post.tags" :key="tag" class="cursor-pointer q-ma-none q-pr-sm">#{{ tag }}</span>
          </p>
          <p class="q-pa-none q-ma-none text-caption text-grey cursor-pointer">View all {{ post.reactions.dislikes.toLocaleString() }} comments</p>
        </q-card-section>
      </q-card>
    </div>
  </q-page>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const stories = ref([]);

const posts = ref([]);

/* Fetch stories */
const fetchStories = async () => {
  try {
    const response = await axios.get('https://randomuser.me/api/?results=10');
    stories.value = response.data.results;
  } catch (error) {
    console.error('Error fetching stories:', error);
  }
};

/* Fetch Posts */
const fetchPosts = async () => {
  try {
    const [postsRes, profilesRes] = await Promise.all([
      axios.get('https://dummyjson.com/posts?limit=10'),
      axios.get('https://randomuser.me/api/?results=10')
    ]);

    const rawPosts = postsRes.data.posts;
    const profiles = profilesRes.data.results;

    // Combine each post with a user profile
    posts.value = rawPosts.map((post, index) => ({
      ...post,
      user: profiles[index] || profiles[0] // fallback to first if not enough profiles
    }));

  } catch (error) {
    console.error('Error fetching posts or profiles:', error);
  }
};

onMounted(() => {
  fetchStories();
  fetchPosts();
});

/* Liked and bookmarked state */
const liked = ref(false);
const saved = ref(false);

const toggleLike = (post) => {
  post.liked = !post.liked;
  post.reactions.likes += post.liked ? 1 : -1;
};

const toggleSave = () => {
  saved.value = !saved.value;
};
</script>
