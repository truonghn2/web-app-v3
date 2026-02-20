<template>
  <v-container class="py-6">
    <div class="section-title">
        <span class="title-text">Boba Drinks</span>
    </div>
    <div
      @mouseenter="isCycling = false"
      @mouseleave="isCycling = true"
    >
      <v-carousel
        v-model="active"
        height="520"
        show-arrows="hover"
        hide-delimiter-background
        hide-delimiters
        :cycle="isCycling"
      >
        <v-carousel-item
          v-for="drink in drinks"
          :key="drink.id"
        >
          <div class="slide-wrap">
          <v-card
            class="mx-auto drink-card"
            max-width="900"
            rounded="xl"
            elevation="4"
            role="button"
            tabindex="0"
            @mouseenter="isCycling = false"
            @mouseleave="isCycling = true"
            @focusin="isCycling = false"
            @focusout="isCycling = true"
            @click="openDrink(drink)"
            @keyup.enter="openDrink(drink)"
          >
            <!-- favorite heart (top-right) -->
            <v-btn
              class="fav-btn"
              icon
              size="small"
              variant="flat"
              @click.stop="toggleFavorite(drink.id)"
              @mousedown.stop
              :aria-label="isFavorite(drink.id) ? 'Unfavorite' : 'Favorite'"
            >
              <v-icon
                :icon="isFavorite(drink.id) ? 'mdi-heart' : 'mdi-heart-outline'" 
                size="26"
              />
            </v-btn>
            <v-img
              :src="drink.image"
              height="320"
              contain
              class="carousel-img"
            >
              <v-chip
                class="ma-3"
                color="#C96B8A"
                variant="flat"
              >
                {{ drink.tag }}
              </v-chip>
            </v-img>

            <v-card-title class="text-h6 font-weight-bold">
              {{ drink.name }}
            </v-card-title>

            <v-card-subtitle>
              {{ drink.description }}
            </v-card-subtitle>

            <v-card-text class="d-flex align-center justify-space-between">
              <div class="text-body-2">
                Base: <strong>{{ drink.base }}</strong>
                <br />
                Topping: <strong>{{ drink.topping }}</strong>
              </div>
              <div class="text-body-1 font-weight-bold">
                ${{ drink.price.toFixed(2) }}
              </div>
            </v-card-text>
          </v-card>
          </div>
        </v-carousel-item>
      </v-carousel>
    </div>

    <!-- custom buttons below -->
    <div class="carousel-dots">
      <v-btn
        v-for="(_, i) in drinks"
        :key="i"
        size="x-small"
        icon
        variant="text"
        class="dot-btn"
        :class="{ active: active === i }"
        :aria-label="`Go to slide ${i + 1}`"
        @click="active = i"
      >
        <v-icon size="15">
          {{ active === i ? 'mdi-circle' : 'mdi-circle-outline' }}
        </v-icon>
      </v-btn>
    </div>

    <!-- Details Dialog-->
    <v-dialog v-model="dialogOpen" max-width="600">
      <v-card rounded="xl">
        <v-img :src="selected?.image" height="260" contain class="dialog-img" />

        <v-card-title class="text-h6 font-weight-bold">
          {{ selected?.name }}
        </v-card-title>

        <v-card-text>
          <div class="mb-2">{{ selected?.description }}</div>

          <v-list density="compact">
            <v-list-item>
              <template #prepend>
                <v-icon>mdi-cup</v-icon>    
              </template>
              <v-list-item-title>Base</v-list-item-title>
              <v-list-item-subtitle>{{ selected?.base }}</v-list-item-subtitle>
            </v-list-item>

            <v-list-item>
              <template #prepend>
                <v-icon>mdi-circle</v-icon>
              </template>
              <v-list-item-title>Topping</v-list-item-title>
              <v-list-item-subtitle>{{ selected?.topping }}</v-list-item-subtitle>
            </v-list-item>

            <v-list-item>
              <template #prepend>
                <v-icon>mdi-cash</v-icon>
              </template>
              <v-list-item-title>Price</v-list-item-title>
              <v-list-item-subtitle>${{ selected?.price?.toFixed(2) }}</v-list-item-subtitle>
            </v-list-item>
          </v-list>
        </v-card-text>

        <v-card-actions class="justify-end">
          <v-btn variant="text" @click="dialogOpen = false">Close</v-btn>
          <!-- <v-btn color="#c96b8a" variant="flat" @click="dialogOpen = false">
            Favorite
          </v-btn> -->
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script setup>
import { ref } from 'vue'

const active = ref(0)

const img = (file) => new URL(`../assets/boba/${file}`, import.meta.url).href

const favorites = ref(new Set())

const isCycling = ref(true)

function isFavorite(id) {
  return favorites.value.has(id)
}

function toggleFavorite(id) {
  const next = new Set(favorites.value) // ensure reactivity updates
  if (next.has(id)) next.delete(id)
  else next.add(id)
  favorites.value = next
}

const drinks = [
  {
    id: 1,
    name: 'Brown Sugar Milk Tea',
    tag: 'Best Seller',
    description: 'Caramel-like brown sugar with creamy milk tea',
    base: 'Milk Tea',
    topping: 'Brown Sugar Boba',
    price: 6.49,
    image: img('brown-sugar.jpg'),
  },
  {
    id: 2,
    name: 'Taro Milk Tea',
    tag: 'Fan Favorite',
    description: 'Sweet, nutty taro flavor with a smooth finish',
    base: 'Milk Tea',
    topping: 'Tapioca Pearls',
    price: 6.29,
    image: img('taro.jpg'),
  },
  {
    id: 3,
    name: 'Matcha Milk Tea',
    tag: 'Creamy',
    description: 'Earthy matcha blended into a milk tea',
    base: 'Matcha',
    topping: 'Tapioca Pearls',
    price: 6.59,
    image: img('matcha.jpg'),
  },
  {
    id: 4,
    name: 'Strawberry Fruit Tea',
    tag: 'Refreshing',
    description: 'Bright strawberry tea with a fruity punch',
    base: 'Fruit Tea',
    topping: 'Lychee Jelly',
    price: 5.99,
    image: img('strawberry.jpg'),
  },
  {
    id: 5,
    name: 'Thai Tea',
    tag: 'Classic',
    description: 'Bold Thai tea with sweet, creamy notes',
    base: 'Thai Tea',
    topping: 'Boba',
    price: 6.19,
    image: img('thai-tea.jpg'),
  },
]

const dialogOpen = ref(false)
const selected = ref(null)

function openDrink(drink) {
  selected.value = drink
  dialogOpen.value = true
}
</script>

<style scoped>
:deep(.v-carousel .v-btn .v-icon) {
  color: #C96B8A;
}

.slide-wrap {
  padding-top: 16px;       
}

.carousel-img :deep(.v-img__img) {
  padding-top: 16px;      
}

.dialog-img :deep(.v-img__img) {
  padding-top: 16px;      
}

.section-title {
  display: inline-block;
  margin-bottom: 12px;
}

.title-text {
  font-size: 1.5rem; 
  font-weight: 700;
  padding-bottom: 6px;
  border-bottom: 3px solid #C96B8A; /* underline */
}

.carousel-dots{
  display: flex;
  justify-content: center;
  gap: 8px;
}

.dot-btn :deep(.v-icon){
  color: rgba(201, 107, 138, 0.45);
}

.dot-btn.active :deep(.v-icon){
  color: #C96B8A;
}

.drink-card{
  position: relative;
}

.fav-btn{
  position: absolute;
  top: 12px;
  right: 12px;
  z-index: 3;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(6px);
}

.fav-btn :deep(.v-icon){
  color: #C96B8A;
}
</style>