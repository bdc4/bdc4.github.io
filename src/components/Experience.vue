<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-timeline>
          <v-timeline-item v-for="card in cards" :key="card.title">
            <template v-slot:opposite>
              <div style="min-height: 200px;">
                <v-img :width="300" aspect-ratio="16/9" cover :src="card.img">
                  <template v-slot:placeholder>
                    <v-row align="center" class="fill-height ma-0" justify="center" transition="fade-transition">
                      <v-progress-circular color="grey lighten-5" indeterminate></v-progress-circular>
                    </v-row>
                  </template>
                </v-img>
              </div>
            </template>
            <div>
              <div class="text-h6">{{ card.title }}</div>
              <div class="text-subtitle-1 text-primary">{{ card.subtitle }}</div>
              <p>
                {{ card.text }}
              </p>
            </div>
          </v-timeline-item>
        </v-timeline>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
const cards = [
  {
    title: 'ServiceNow Consulting',
    subtitle: '10+ Years',
    text: 'Bryce has been doing a lot of ServiceNow consulting, especially Service Portal. Lead portal developer on the Now Learning 2.0 UI Framework rewrite.',
    img: '../src/assets/servicenow-logo.png'
  },
  {
    title: 'Web Development',
    subtitle: '12+ Years',
    text: 'Responsible for several large, public facing websites, using Azure and AWS. Custom web apps, including Discord bots and webhooks. Sites include SW5e.com and n7.world',
    img: '../src/assets/techlogos.png'
  },
  {
    title: '3D Modeling/Printing',
    subtitle: '5+ Years',
    text: 'Specializing in functional printing and miniature printing, I will work with you to figure out what you need and draft up a part.',
    img: '../src/assets/3dprinting-logo.jpg'
  }
]
</script>