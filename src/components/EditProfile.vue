<script setup>
import { ref } from 'vue'

const profileName = ref(null)
const profile = ref(null)

const fetchProfile = () => {
    fetch(`http://timoth.yt/api/rampup?name=${profileName.value}`, {
        method: 'GET',
        headers: {
            'Content-Type': 'application/json',
        },
    })
    .then(response => response.json())
    .then(data => profile.value = data)
}

/* ADD JAVASCRIPT CODE HERE */

</script>

<template>
    <h1>Edit Profile</h1>
    <form id="fetchProfileForm" onsubmit="return false">
        <label for="profileName">Profile Name:</label>
        <input id="profileName" v-model="profileName" required>
        <input type="submit" @click="fetchProfile()">
    </form>

    {{ profile }}

</template>

<style scoped>

#fetchProfileForm input {
    margin: 5px;
    background-color: #d1d1d1;
    border-radius: 5px;
}

/* EDIT CSS STYLING HERE */

</style>
