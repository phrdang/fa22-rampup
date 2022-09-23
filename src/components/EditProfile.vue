<script setup>
import { ref } from 'vue'
 
const profileName = ref(null)
const profile = ref(null)
let name
let age
let link
let bio
let likes


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

const renderProfile = () => {
    fetchProfile()

    console.log(profile)

    console.log("profile.Name")
    console.log(profile.Name)

    name = ref(profile.Name)
    age = ref(profile.Age)
    link = ref(profile.Link)
    bio = ref(profile.Bio)
    likes = ref(profile.Likes)
    console.log(name)
    console.log(age.value)
    console.log(link.value)
    console.log(bio.value)
    console.log(likes.value)
}

const updateProfile = () => {
    // TODO initialize newProfileObject
    fetch(`http://timoth.yt/api/rampup?name=${profileName.value}`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
		body: JSON.stringify(newProfileObject)
    })
}

</script>

<template>
    <h1>Edit Profile</h1>
    <form id="fetchProfileForm" onsubmit="return false">
        <label for="profileName">Profile Name:</label>
        <input id="profileName" v-model="profileName" required>
        <input type="submit" @click="renderProfile()">
    </form>

    {{ profile }}

    <!--TODO only render form once fetched profile-->

    <!-- This form tag wraps our inputs & allows us to require certain fields prior to submission -->
    <div v-if="profile" class="form-wrapper">
        <form id="editProfileForm" onsubmit="return false">
            <!-- These divs are for formatting to allow these inputs to sit side-by-side -->
            <div class='sub-form'>
                <div class='pair'>
                    <!-- Input & Label for 'Name' attribute -->
                    <label for="name">Name:</label>
                    <input id="name" v-model="name" placeholder="First Name" required>
                </div>
                <div class='pair'>
                    <!-- Input & Label for 'Age' attribute -->
                    <label for="age">Age:</label>
                    <input id="age" v-model="age" placeholder="Optional (put 0)" required>
                </div>
            </div>

            <!-- Optional Input & Label for 'Link' attribute -->
            <label for="link">Link:</label>
            <input id="link" v-model="link" placeholder="Social Media URL (optional)">

            <!-- Input & Label for 'Bio' attribute -->
            <label for="bio">Bio:</label>
            <textarea id="bio" v-model="bio" placeholder="A couple sentences about you..." required />

            <!-- Inputs & Labels for 'Likes' attribute -->
            <label for="likes">Likes:</label>
            <div id="likes" v-for="index in likeCount" v-bind:key="index">
                <!-- This block dynamically generates a number of input pairs 
                for each Like according to the value of likeCount -->
                <input v-model="likes[index - 1].category" placeholder="Category" required>
                <input v-model="likes[index - 1].favorite" placeholder="Your Favorite" required>
            </div>
            <!-- Buttons for submission & adding more Like inputs -->
            <div class='sub-form'>
                <input class='form-button' type="submit" @click="updateProfile()">
            </div>
        </form>
    </div>

</template>

<style scoped>

#fetchProfileForm input {
    margin: 5px;
    background-color: #d1d1d1;
    border-radius: 5px;
}

.form-wrapper {
  width: 520px;
  display: flex;
  flex-direction: column;
}

.sub-form {
    width: 100%;
    display: flex;
    flex-direction: row;
}

.pair {
    display: flex;
    flex-direction: column;
}

.pair input {
    min-width: 250px;
}

.remove-like {
    width: 25px;
    height: 25px;
    background-color: rgb(255, 152, 152);
}

.form-button {
    width: 100px;
    height: 40px;
}

form {
    display: flex;
    flex-direction: column;
}

input {
    height: 30px;
}

textarea {
    height: 100px;
}

input, textarea, button {
    background-color: #d1d1d1;
    border-radius: 5px;
    margin: 10px 10px 10px 0;
}


</style>
