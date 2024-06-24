<template>
  <div
    class="star-rating"
    @mousemove="handleEvent"
    @click="handleEvent"
    @mouseleave="handleEvent"
  >
    <span
      v-for="star in 5"
      :key="star"
      class="star"
      :class="{ 'star--filled': star <= currentHoverRating }"
      :data-star="star"
    >
      &#9733;
    </span>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue"

const emits = defineEmits(["ratingSelected"])

const rating = ref(0)
const currentHoverRating = ref(0)

const handleEvent = event => {
  const starRating = parseInt(event.target.dataset.star)

  switch (event.type) {
    case "click":
      if (starRating) {
        rating.value = starRating
        currentHoverRating.value = starRating
        emits("ratingSelected", starRating)
      }
      break
    case "mousemove":
      if (starRating) {
        currentHoverRating.value = starRating
      }
      break
    case "mouseleave":
      currentHoverRating.value = rating.value
      break
  }
}
</script>

<style scoped>
.star-rating {
  display: flex;
  cursor: pointer;
}

.star {
  font-size: 2rem;
  color: #ccc;
}

.star--filled {
  color: gold;
}
</style>
