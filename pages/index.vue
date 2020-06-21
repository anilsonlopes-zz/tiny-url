<template>
  <div class="mx-auto min-h-screen px-4 flex justify-center items-center">
    <div>
      <logo />
      <h1 class="text-center my-4 text-4xl text-gray-900">
        tiny-url
      </h1>
      <h2 class="text-center text-lg text-gray-700">
        Shorten your links with vue/nuxtjs and firebase
      </h2>
      <form class="pt-10" @submit.prevent="generate">
        <input
          v-model="url"
          type="text"
          placeholder="A long url"
          class="w-full p-2 border rounded shadow text-center focus:shadow-outline focus:outline-none"
        />
        <button
          type="submit"
          class="p-2 w-full rounded bg-gray-200 focus:shadow-outline focus:outline-none mt-2"
        >
          Generate
        </button>
        <div v-if="tiny">
          <nuxt-link class="tiny" :to="{ name: 'id', params: { id: tiny } }">
            {{ tiny }}
          </nuxt-link>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import firebase from '~/firebase'
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  data() {
    return {
      url: '',
      tiny: ''
    }
  },
  methods: {
    async generate() {
      const doc = await firebase
        .firestore()
        .collection('tiny-url')
        .add({
          url: this.url,
          createAt: firebase.firestore.FieldValue.serverTimestamp()
        })
      this.tiny = doc.id
    }
  }
}
</script>
