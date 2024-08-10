<script setup lang="ts">
import landingPic from "../assets/demonsnake.png";
import { ref } from "vue";
import emailjs from "emailjs-com";

// Define reactive variables
const characterName = ref("");
const faction = ref("");
const boostType = ref("");
const successMessage = ref("");
const errorMessage = ref("");

// Function to send email
const sendEmail = () => {
  const templateParams = {
    character_name: characterName.value,
    faction: faction.value,
    boost_type: boostType.value,
    to_email: "dahlbergjakob@live.se",
  };

  emailjs
    .send("DemonSnake", "template_48w6lqv", templateParams, "HQv9q-RWO7QpQ0sqD")
    .then(
      (response) => {
        console.log("SUCCESS!", response.status, response.text);
        successMessage.value = "Your request has been sent champion";
        errorMessage.value = "";

        setTimeout(() => {
          characterName.value = "";
          faction.value = "";
          boostType.value = "";
          successMessage.value = "";
        }, 5000);
      },
      (error) => {
        console.log("FAILED...", error);
        successMessage.value = "";
        errorMessage.value = "Boosting service not available right now";

        setTimeout(() => {
          errorMessage.value = "";
        }, 5000);
      }
    );
};
</script>

<template>
  <div
    class="relative flex items-center justify-center min-h-screen bg-gray-800"
  >
    <img
      :src="landingPic"
      class="absolute top-0 left-0 h-full w-full object-cover opacity-50"
    />
    <h1
      class="absolute top-10 left-1/2 transform -translate-x-1/2 text-4xl font-bold text-gradient"
    >
      The Fastest MoP Remix Booster on the REALM
    </h1>
    <form
      @submit.prevent="sendEmail"
      class="relative bg-purple-900 bg-opacity-30 p-8 rounded-lg shadow-lg w-full max-w-md border-4 border-red-600"
    >
      <div class="mb-4">
        <label
          for="characterName"
          class="block text-sm font-medium text-gray-200"
          >Character Name:</label
        >
        <input
          type="text"
          id="characterName"
          v-model="characterName"
          required
          class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-red-500 focus:border-red-500 sm:text-sm bg-transparent text-white"
        />
      </div>
      <div class="mb-4">
        <label for="faction" class="block text-sm font-medium text-gray-200"
          >Alliance or Horde:</label
        >
        <select
          id="faction"
          v-model="faction"
          required
          class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-red-500 focus:border-red-500 sm:text-sm bg-transparent text-white"
        >
          <option value="Alliance" class="text-black">Alliance</option>
          <option value="Horde" class="text-black">Horde</option>
        </select>
      </div>
      <div class="mb-4">
        <label for="boostType" class="block text-sm font-medium text-gray-200"
          >Raid boost or lvl boost:</label
        >
        <input
          type="text"
          id="boostType"
          v-model="boostType"
          required
          class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-red-500 focus:border-red-500 sm:text-sm bg-transparent text-white"
        />
      </div>
      <button
        type="submit"
        class="w-full py-2 px-4 bg-red-700 text-white font-medium rounded-md shadow-sm hover:bg-red-800 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
      >
        Submit
      </button>
      <p v-if="successMessage" class="mt-4 text-green-600">
        {{ successMessage }}
      </p>
      <p v-if="errorMessage" class="mt-4 text-red-600">{{ errorMessage }}</p>
    </form>
  </div>
</template>

<style scoped>
form {
  background: rgba(
    75,
    0,
    130,
    0.3
  ); /* Deep purple with increased transparency */
  border: 2px solid red; /* Red border */
  border-radius: 10px;
  box-shadow: 0 0 10px red; /* Red glowing edges */
}

button {
  background-color: #8b0000; /* Dark red */
  color: white;
}

button:hover {
  background-color: #b22222; /* Slightly lighter dark red */
}

input,
select {
  background-color: rgba(255, 255, 255, 0.1); /* Transparent input background */
  color: white;
}

label {
  color: white;
}

.text-gradient {
  background: linear-gradient(90deg, purple, red);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
