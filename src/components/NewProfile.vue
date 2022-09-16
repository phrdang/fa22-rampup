<script setup>
import { ref } from 'vue'

/* These ref values track each part of our profile & automatically update
 * when their corresponding inputs are edited (v-model). We also track the
 * # of likes through likeCount, as well as a value for the response from
 * the server that allows us to automatically display it on the page.
 */
const name = ref('')
const link = ref(null)
const age = ref(null)
const bio = ref('')
const likes = ref([{ 'category': '', 'favorite': '' }])
const likeCount = ref(1)
const serverResponse = ref(null)

// This function resets our profile-related refs / clears the form
const resetForm = () => {
    name.value = ''
    link.value = null
    age.value = null
    bio.value = ''
    likes.value = [{ 'category': '', 'favorite': '' }]
    likeCount.value = 1
}

// This function adds a Like & provisions space for it in the 'likes' ref
const addLike = () => {
    likeCount.value++
    likes.value[likeCount.value - 1] = {
        'category': '',
        'favorite': ''
    }
}

// This function removes the last Like that was added
const removeLike = () => {
    likes.value.pop()
    likeCount.value--
}

// This function converts our 'likes' ref to the format necessary for our API call
const importLikes = (likes) => {
    const likesObject = {}
    likes.value.forEach((like) => {
        likesObject[like.category] = like.favorite
    })
    return likesObject
}

/* This function is called upon form submission. It takes the current ref values,
 * packages them into a body object (converting the likes along the way), and sends
 * them to the FakeBook server to create a profile. It then stores the response to be
 * displayed and resets the form.
 */
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
    <!-- Page Title -->
    <h1>New Profile</h1>

    <!-- This form tag wraps our inputs & allows us to require certain fields prior to submission -->
    <form id="newProfileForm" onsubmit="return false">
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
            <!-- The removal button appears next to the last additional Like -->
            <button class="remove-like" v-if="index === likeCount && index > 1" @click="removeLike()">X</button>
        </div>
        <!-- Buttons for submission & adding more Like inputs -->
        <div class='sub-form'>
            <input class='form-button' type="submit" @click="makeNewProfile()">
            <button class='form-button' @click="addLike()">Add Like</button>
        </div>
    </form>

    <!-- Displays the response message from the server after an API call -->
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
