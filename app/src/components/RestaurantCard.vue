<script setup lang="ts">
import type { Restaurant } from '@/types'
import { defineProps, computed } from 'vue'
import type { PropType } from 'vue'

// const props = defineProps({
//   restaurant: {
//     type: Object as PropType<Restaurant>,
//     required: true,
//     default: {
//       id: '1',
//       name: 'Restaurant Name',
//       status: 'Want to Try',
//       address: '1234 Main St, City, State 12345',
//     },
//   },
// })

// ------------------or-----------------------

// As of Vue 3.2.21, the defineProps function does not support the required property. Inside typescrypt as long as you define a property on an object, it's by default required. Put ?: to make it optional.
type PropTypes = {
  restaurant?: Restaurant
}
const props = defineProps<PropTypes>()

// -------------------or----------------------

// // to use default values, you can use useWithDefaults.
// useWithDefaults(defineProps<PropTypes>(), {
//   restaurant: {
//     default: {
//       id: '1',
//       name: 'Restaurant Name',
//       status: 'Want to Try',
//       address: '1234 Main St, City, State 12345',
//     },
//   },
// })

// -------------------or----------------------

//See also https://vuejs.org/guide/typescript/composition-api.html#typing-components-props

// emits: ['delete-restaurant'],

const statusColor = computed(() => {
  if (!props.restaurant) return ''
  switch (props.restaurant.status) {
    case 'Want to Try':
      return 'is-warning'
    case 'Recommended':
      return 'is-success'
    case 'Do Not Recommend':
      return 'is-danger'
    default:
      return ''
  }
})

const deleteRestaurant = () => {
  // this.$emit('delete-restaurant', this.restaurant)
}
</script>

<template>
  <article class="box">
    <div class="media">
      <aside class="media-left">
        <img src="https://placehold.jp/150x150.png" alt="" />
      </aside>
      <div class="media-content">
        <p class="title is-4 is-spaced mb-1">
          {{ props.restaurant?.name }}
        </p>
        <p class="subtitle mb-2">
          <span class="tag" :class="statusColor">{{ props.restaurant?.status }}</span>
        </p>
        <div class="content mb-2">
          {{ props.restaurant?.address }}
        </div>
        <div>
          <button @click="deleteRestaurant" class="button is-small is-danger is-light">Delete</button>
        </div>
      </div>
    </div>
  </article>
</template>

<style></style>
