<script setup>
defineProps({
  profile: {
    type: Object,
    required: true
  }
})

</script>

<template>
  <div class="card">
    <h1>{{ profile.Name }}</h1>
    <h3>{{ profile.Age }}</h3>
    <div v-if="profile.Link">
      <a :href="profile.Link">{{ profile.Link }}</a>
    </div>
    <h3>{{ profile.Bio }}</h3>
    <br>
    <h2>Likes</h2>
    <li v-for="like in Object.keys(profile.Likes)" v-bind:key="like">
      {{ like }}: {{ profile.Likes[like] }}
    </li>
    <br>
  </div>
</template>

<style scoped>

.card {
  display: flex;
  flex-direction: column;
  width: 450px;
  height: 450px;
  overflow: hidden;
  overflow-y: auto;
  padding: 20px;
  margin: 10px;
  background:#404040;
}

</style>
