<template>
  <v-row class="text-center">
    <v-col cols="12">
      <div class="omni-logo-container"></div>
      <div class="text-body-2 font-weight-light mb-n1">Owned and operated by Bryce Gelinas</div>
    </v-col>
  </v-row>

  <v-row>
    <v-col cols="12">
      <v-tabs v-model="tab" bg-color="bg-primary" align-tabs="center" stacked>
        <v-tab :value="1">
          <v-icon>mdi-file-account</v-icon>
          Experience
        </v-tab>

        <v-tab :value="2">
          <v-icon>mdi-tools</v-icon>
          Projects
        </v-tab>

        <v-tab :value="3">
          <v-icon>mdi-message-outline</v-icon>
          Contact Info
        </v-tab>
      </v-tabs>

      <v-window v-model="tab">
        <v-window-item :value="1">
          <Experience />
        </v-window-item>
        <v-window-item :value="2">
          Test
        </v-window-item>
        <v-window-item :value="3">
          Test
        </v-window-item>
      </v-window>
    </v-col>
  </v-row>
</template>

<script setup>
import { ref } from 'vue';
const tabs = [];
const tab = ref(1);
</script>

<style scoped>
.omni-logo-container {
  height: 100px;
  overflow: auto;
  background-image: url('../src/assets/logo.svg?t=1705878810487');
  background-position: center;
  background-size: 300px;
}
</style>