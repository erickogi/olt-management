<template>
  <v-app>
    <v-main>
      <OltForm />
    </v-main>
  </v-app>
</template>

<script>
import OltForm from './components/OltForm.vue'

export default {
  name: 'App',
  components: {
    OltForm
  }
}
</script>