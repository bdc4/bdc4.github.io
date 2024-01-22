<template>
  <v-app dark>
    <v-main>
      <Main />
    </v-main>
    <v-footer app class="bg-primary-lighten-1 text-center d-flex flex-column">
      <div>
        <v-btn color="#0a66c2" href="https://www.linkedin.com/in/bryce-cloke/" target="_blank">
          <v-icon class="mx-2">mdi-linkedin</v-icon>
          Reach out to me on LinkedIn!
        </v-btn>
      </div>

      <div class="pt-5">
      </div>

      <v-divider></v-divider>

      <div>
        <span>©{{ new Date().getFullYear() }} — <strong>OmniBrew LLC</strong></span>
        <span><v-btn class="mx-2" text icon="mdi-github"></v-btn></span>
      </div>
    </v-footer>
  </v-app>
</template>

<script setup>
const icons = [
  {
    mdi: 'mdi-linkedin',
    url: 'https://www.linkedin.com/in/bryce-cloke/'
  },
  {
    mdi: 'mdi-github',
    url: 'https://www.github.com/bdc4'
  }
]
</script>
