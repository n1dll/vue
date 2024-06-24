<template>
  <StarRating @ratingSelected="openModal" />
  <RatingModal
    :initialRating="selectedRating"
    :showModal="isModalVisible"
    @update:showModal="isModalVisible = $event"
  />
</template>

<script setup>
import RatingModal from "./components/RatingModal.vue"
import StarRating from "./components/StarRating.vue"

import { ref } from "vue"

const selectedRating = ref(0)
const isModalVisible = ref(false)

const openModal = rating => {
  selectedRating.value = rating
  isModalVisible.value = true
}
</script>
