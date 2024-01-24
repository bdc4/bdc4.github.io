<template>
  <div class="d-flex align-center">
    <div class="mr-2">Filters:</div>
    <v-chip-group v-model="filter" multiple>
      <template v-for="(item, index) in allFilters" :key="index">
        <v-chip filter variant="outlined" :value="item">{{ item }}</v-chip>
      </template>
    </v-chip-group>
  </div>
  <template v-if="filteredCards.length">
    <v-card v-for="(card, index) in filteredCards" :key="card.title" class="my-2">
      <v-card-title v-if="!card.image">
        {{ card.title }}
      </v-card-title>
      <v-card-text :style="card.styles">
        <div class="d-flex">
          <v-img height="50" aspect-ratio="16/9" :src="card.image" :alt="card.title"></v-img>
        </div>

        <div class="mt-5"
          @click="card.typedText.length < CHAR_MAX ? typeOutText(card, card.text.slice(0, CHAR_MAX)) : typeOutText(card)">
          <div v-if="card.typedText.length" :style="`height: ${card.refHeight || getRefHeight(`typingRef${index}`, card)}px;`">
            <div
              :style="`visibility: hidden; position: absolute; display: ${card.typedText.length == card.text.length ? 'none' : 'inherit'}`"
              :class="`typingRef${index}`"><div>{{ card.text }}</div><br /></div>
            <div>
              <div :class="(!card.typingDone) ? 'blinker' : ''">
                {{ card.typedText }}
              </div>

              <a v-if="card.typedText" class="text-primary">
                <br />
                <span v-if="card.typedText.length == CHAR_MAX" @click.stop="typeOutText(card)">
                  ...show more</span>
                <span v-if="card.typedText.length == card.text.length && card.text.length > CHAR_MAX"
                  @click.stop="(typeOutText(card, card.text.slice(0, CHAR_MAX)))">
                  ...show less</span>
              </a>
            </div>

          </div>
        </div>
      </v-card-text>
      <v-card-actions class="flex-column align-end">
        <div class="align-end d-flex flex-row justify-sm-space-between w-100">
          <!-- {{ (card.typing ? 'typing...' : card.typingDone ? 'Done!' : 'Almost done...') }} -->
          <v-progress-circular size="small" class="ml-2"
            :color="`rgba(${255 + (255 * (-card.percent / 100))},${255 * (card.percent / 100)},0,1)`"
            :model-value="card.percent"></v-progress-circular>
          <v-btn v-if="card.href" append-icon="mdi-open-in-new" class="mt-4" :href="card.href" target="_blank"
            variant="outlined" color="primary">
            See for yourself!
          </v-btn>
        </div>
        <div style="width: 100%;" class="my-2">
          <v-divider :thickness="2"></v-divider>
        </div>

        <v-chip-group>
          <template v-for="(item, index) in getMetaArray(card.meta)" :key="index">
            <v-chip variant="outlined" size="x-small">{{ item }}</v-chip>
          </template>
        </v-chip-group>

      </v-card-actions>
    </v-card>
  </template>
  <div v-else class="h3 mx-auto">
    No Items Found
  </div>
</template>

<script setup>
import { ref, reactive, computed, watch } from 'vue';
var filter = ref([]);

const delay = ms => new Promise(res => setTimeout(res, ms));

const getRefHeight = async (refKey, card) => {
  try {
    const _ref = document.querySelectorAll(`.${refKey}`)[0] //refs.value[refKey][0];
    const refHeight = window.getComputedStyle(_ref, null).getPropertyValue('height');
    card.refHeight = refHeight.replace("px", "");
    return card.refHeight;
  } catch (e) {
    return "0";
  }
}

const CHAR_MAX = 1000;

const typeOutText = async (card, typedTextOverride) => {

  const LETTER_DELAY = 10;
  const PUNC_DELAY = 200;

  card.typedText = typedTextOverride || card.typedText || '';
  var i = card.typedText.length;
  card.typingDone = false;
  card.percent = (100 * (card.typedText.length / card.text.length));

  var lengthLimit = (!card.readMore && card.text.length >= CHAR_MAX) ? CHAR_MAX : card.text.length;
  if (i < (lengthLimit)) {
    card.typing = true;
    i++;
    card.typedText = card.text.slice(0, i);
    var DELAY = ['.', '?', ';', '!']
      .includes(card.typedText[card.typedText.length - 1])
      ? PUNC_DELAY : LETTER_DELAY;
    await delay(DELAY);
    return typeOutText(card);
  } else if (card.typedText.length == CHAR_MAX) {
    card.readMore = true;
  } else {
    card.readMore = false;
  }
  card.typing = false;
  if (card.typedText.length == card.text.length) {
    card.typingDone = true;
  }
}

const cards = ref([
  {
    title: 'Mass Effect 5e',
    text: `Mass Effect 5e is a homebrew sci-fi system for the Dungeons & Dragons 5th Edition. 
      Written in Vue/Vuex and hosted through AWS, it functions as a wholely self-contained system, 
      including character creation, character sheet management, searchable rules, and more. 
      It even includes functionality for collaborative, online dice-roll sharing through Discord webhooks, 
      forwarding the dice rolls directly from your online character sheet to your game's Discord server!`,
    typedText: '',
    image: 'src/assets/me5e-logo.png',
    href: 'https://www.n7.world',
    meta: 'Personal,Games,D&D,Discord',
    styles: {
      fontFamily: 'monospace',
      color: 'red'
    }
  },
  {
    title: 'Star Wars 5e',
    text: 'Hello There!',
    typedText: '',
    image: 'src/assets/sw5e-logo.png',
    href: 'https://www.sw5e.com',
    meta: 'Personal,Games,D&D'
  }
]);

/*
watch(cards, async (newValue, oldValue) => {
  debugger;
  newValue.forEach(card => {
    card = typeOutText(card)
  })
}, { deep: true });
*/

cards.value.forEach(card => {
  card.typedText = card.text[0];
  card = typeOutText(card);
})

const getMetaArray = (m) => {
  return ((typeof m == 'string') ? m.split(',') : m);
}

const allFilters = computed(() => {
  var arr = [];
  cards.value.forEach(card => {
    var m = getMetaArray(card.meta);
    m.forEach(i => {
      if (!arr.includes(i)) {
        arr.push(i);
      };
    });
  });
  return arr;
});

const filteredCards = computed(() => {
  var _filteredCards = [];
  cards.value.forEach(card => {
    if (!filter.value.length || (card.meta && filter.value.some(v => {
      var m = getMetaArray(card.meta);
      return m.includes(v)
    }))) {
      _filteredCards.push(card);
    }
  });
  return _filteredCards;
})
</script>

<style scoped>
.blinker:after {
  content: '_';
  display: inline-block;
  animation: blink 1s infinite;
  font-weight: bold;
}

@keyframes blink {
  0% {
    opacity: 1;
  }

  1% {
    opacity: 0;
  }

  50% {
    opacity: 0;
  }

  51% {
    opacity: 1;
  }
}</style>