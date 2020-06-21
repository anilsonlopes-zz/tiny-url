<template>
  <div>Redirecionando...</div>
</template>

<script>
import firebase from '~/firebase'

export default {
  async asyncData({ redirect, params }) {
    const doc = await firebase
      .firestore()
      .collection('tiny-url')
      .doc(params.id)
      .get()
    const data = doc.data()
    if (doc.exists) {
      redirect(302, data.url)
    }
    return {
      document: data
    }
  }
}
</script>
