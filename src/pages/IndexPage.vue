<template>
  <q-page class="feed">
    <!-- Story section -->

    <q-scroll-area :visible="false" class="story-section">
      <div style="height: 100%" class="row no-wrap items-center q-gutter-md">
        <q-avatar class="user-story-avatar" size="65px">
          <q-img src="/src/assets/headshot.jpg" />
          <q-btn class="add-story-btn" flat rounded dense icon="bi-plus" />
        </q-avatar>
        <div class="ring" v-for="n in 10" :key="n">
          <div class="inner-ring">
            <q-avatar size="70px">
              <q-img src="/src/assets/headshot.jpg" />
            </q-avatar>
          </div>
        </div>
      </div>
    </q-scroll-area>

    <!-- Feed section -->

    <div v-for="n in 50" :key="n">

    </div>
  </q-page>
</template>
