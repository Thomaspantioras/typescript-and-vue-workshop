<script setup lang="ts">
import { onMounted, ref } from 'vue'
import NewRestaurantForm from '../components/NewRestaurantForm.vue'
import RestaurantCard from '../components/RestaurantCard.vue'
import SideMenu from '../components/SideMenu.vue'
import type { Restaurant } from '@/types'
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { useRestaurantStore } from '@/stores/RestaurantStore'

/**
 * Restaurant module
 */
const restaurantStore = useRestaurantStore()
const restaurantList = restaurantStore.list
// const restaurantList = ref<Restaurant[]>([])

const filterText = ref('')
const filteredRestaurantList = computed((): Restaurant[] => {
  return restaurantList.filter((restaurant: Restaurant) => {
    if (restaurant.name) {
      return restaurant.name.toLowerCase().includes(filterText.value.toLowerCase())
    } else {
      return restaurantList
    }
  })
})

// moved to store
// const numberOfRestaurants = computed((): number => {
//   return filteredRestaurantList.value.length
// })

// const addRestaurant = (payload: Restaurant) => {
//   restaurantList.push(payload)
//   hideForm()
// }
const addRestaurant = (payload: Restaurant) => {
  restaurantStore.addRestaurant(payload)
  hideForm()
}

// const deleteRestaurant = (payload: Restaurant) => {
//   restaurantList.value = restaurantList.value.filter((restaurant: Restaurant) => {
//     return restaurant.id !== payload.id
//   })
// }

/**
 * New form module
 */
const showNewForm = ref(false)
const hideForm = () => {
  showNewForm.value = false
}
onMounted(() => {
  const route = useRoute()

  if (route.query.new) {
    showNewForm.value = true
  }
})
</script>

<template>
  <main class="section">
    <div class="columns">
      <!-- Side Menu -->
      <SideMenu />

      <!-- Main Content -->
      <div class="column">
        <h1 class="title">Restaurants</h1>

        <!-- CTA Bar -->
        <nav v-if="!showNewForm" class="level">
          <div class="level-left">
            <div class="level-item">
              <p class="subtitle is-5">
                <strong>{{ restaurantStore.numberOfRestaurants }}</strong> restaurants
              </p>
            </div>

            <p class="level-item">
              <button @click="showNewForm = true" class="button is-success">New</button>
            </p>

            <div class="level-item is-hidden-tablet-only">
              <div class="field has-addons">
                <p class="control">
                  <input class="input" type="text" placeholder="Restaurant name" v-model="filterText" />
                </p>
                <p class="control">
                  <button class="button">Search</button>
                </p>
              </div>
            </div>
          </div>
        </nav>

        <!-- New Restaurant Form -->
        <NewRestaurantForm v-if="showNewForm" @add-new-restaurant="addRestaurant" @cancel-new-restaurant="hideForm" />

        <!-- Display Results -->
        <div v-else class="columns is-multiline">
          <div v-for="item in filteredRestaurantList" class="column is-full" :key="`item-${item}`">
            <RestaurantCard :restaurant="item" @delete-restaurant="restaurantStore.deleteRestaurant" />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style></style>
