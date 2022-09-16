<script setup>
import Profiles from './components/Profiles.vue'
import NewProfile from './components/NewProfile.vue'
import EditProfile from './components/EditProfile.vue'
import { ref } from 'vue'

const pageLoaded = ref('view-profiles')

const setPage = (newPage) => {
  pageLoaded.value = newPage
}

</script>

<template>
<div class='wrapper'>
    <div class="topnav">
    <a @click="setPage('view-profiles')">Profiles</a>
    <a @click="setPage('new-profile')">New Profile</a>
    <a @click="setPage('edit-profile')">Edit Profile</a>
  </div>
  <div class='content'>
    <div v-if="pageLoaded === 'edit-profile'">
      <EditProfile />
    </div>
    <div v-else-if="pageLoaded === 'new-profile'">
      <NewProfile />
    </div>
    <div v-else>
      <Profiles />
    </div>
  </div>
</div>

</template>

<style scoped>

.wrapper {
  width: 100%;
  display: flex;
  flex-direction: column;
}

.content {
  padding: 2rem;
}

.topnav {
  background-color: #333;
  overflow: hidden;
}

.topnav a {
  float: left;
  color: #f2f2f2;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
  font-size: 17px;
}

.topnav a:hover {
  background-color: rgb(150, 150, 150);
  color: black;
  cursor: pointer;
}
  
</style>
