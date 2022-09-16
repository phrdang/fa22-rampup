<script setup>
import { ref } from 'vue'

const name = ref('')
const link = ref(null)
const age = ref(null)
const bio = ref('')
const likes = ref([{ 'category': '', 'favorite': '' }])
const likeCount = ref(1)
const serverResponse = ref(null)

const resetForm = () => {
    name.value = ''
    link.value = null
    age.value = null
    bio.value = ''
    likes.value = [{ 'category': '', 'favorite': '' }]
    likeCount.value = 1
}

const addLike = () => {
    likeCount.value++
    likes.value[likeCount.value - 1] = {
        'category': '',
        'favorite': ''
    }
}

const removeLike = () => {
    likes.value.pop()
    likeCount.value--
}

const importLikes = (likes) => {
    const likesObject = {}
    likes.value.forEach((like) => {
        likesObject[like.category] = like.favorite
    })
    return likesObject
}

const makeNewProfile = () => {
    const body = {
        'Name': name.value,
        'Age': age.value,
        'Bio': bio.value,
        'Likes': importLikes(likes)
    }
    if (link) {
        body['Link'] = link.value
    }

    fetch(`http://timoth.yt/api/rampup?name=${name.value}`, {
        method: 'PUT',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(body)
    })
    .then(response => response.json())
    .then(data => serverResponse.value = data.message)

    resetForm()
}
</script>

<template>
  <div class='form-wrapper'>
    <h1>New Profile</h1>
    <form id="newProfileForm" onsubmit="return false">
        <div class='sub-form'>
            <div class='pair'>
                <label for="name">Name:</label>
                <input id="name" v-model="name" placeholder="First Name" required>
            </div>
            <div class='pair'>
                <label for="age">Age:</label>
                <input id="age" v-model="age" placeholder="Optional (put 0)" required>
            </div>
        </div>

        <label for="link">Link:</label>
        <input id="link" v-model="link" placeholder="Social Media URL (optional)">

        <label for="bio">Bio:</label>
        <textarea id="bio" v-model="bio" placeholder="A couple sentences about you..." required />

        <label for="likes">Likes:</label>
        <div id="likes" v-for="index in likeCount" v-bind:key="index">
            <input v-model="likes[index - 1].category" placeholder="Category" required>
            <input v-model="likes[index - 1].favorite" placeholder="Your Favorite" required>
            <button class="remove-like" v-if="index === likeCount && index > 1" @click="removeLike()">X</button>
        </div>
        <div class='sub-form'>
            <input class='form-button' type="submit" @click="makeNewProfile()">
            <button class='form-button' @click="addLike()">Add Like</button>
        </div>
    </form>
    <div v-if="serverResponse">
        Response: {{ serverResponse }}
    </div>
  </div>

</template>

<style scoped>
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
