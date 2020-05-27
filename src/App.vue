<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-toolbar-title>Mail Client</v-toolbar-title>
      </div>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-content>
      <SendMail />
    </v-content>
  </v-app>
</template>

<script>
import SendMail from './components/SendMail.vue'

export default {
  name: 'App',
  components: { SendMail },
  data: () => ({
    //
  })
}
</script>
