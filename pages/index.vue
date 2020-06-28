<template>
  <div
    class="mx-auto min-h-screen px-4 flex justify-center items-center bg-gray-200"
  >
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
          class="p-2 w-full rounded text-white bg-green-500 focus:shadow-outline focus:outline-none mt-2"
        >
          Generate
        </button>
        <div v-if="tiny" class="pt-2">
          <nuxt-link
            class="text-center block p-2 rounded bg-green-800 text-xs"
            target="_blank"
            :to="{ name: 'id', params: { id: tiny } }"
          >
            <span class="text-gray-200">{{ baseUrl }}</span
            ><span class="text-white">{{ tiny }}</span>
          </nuxt-link>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { nanoid } from 'nanoid'
import firebase from '~/firebase'
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  data() {
    return {
      baseUrl: location.host + location.pathname,
      url: '',
      tiny: ''
    }
  },
  methods: {
    async generate() {
      if (this.url.trim().length <= 1) {
        return false
      }

      const id = nanoid(6)
      await firebase
        .firestore()
        .collection('tiny-url')
        .doc(id)
        .set({
          url: this.url,
          createAt: firebase.firestore.FieldValue.serverTimestamp()
        })
      this.tiny = id
    }
  }
}
</script>
