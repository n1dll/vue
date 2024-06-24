<template>
  <div
    v-if="isVisible"
    class="modal-overlay"
  >
    <div class="modal">
      <button
        class="close-button"
        @click="closeModal"
      >
        X
      </button>
      <h2>Rate Us</h2>
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
      <textarea
        v-model="feedback"
        placeholder="Write your feedback here"
      ></textarea>
      <button @click="submitFeedback">Submit</button>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, defineProps, defineEmits } from "vue"

const props = defineProps({
  initialRating: {
    type: Number,
    default: 0,
  },
  showModal: {
    type: Boolean,
    default: false,
  },
})

const emits = defineEmits(["update:showModal", "submit"])

const isVisible = ref(props.showModal)
const rating = ref(props.initialRating)
const currentHoverRating = ref(props.initialRating)
const feedback = ref("")

watch(
  () => props.showModal,
  newVal => {
    isVisible.value = newVal
  }
)

watch(
  () => props.initialRating,
  newVal => {
    rating.value = newVal
    currentHoverRating.value = newVal
  }
)

const handleEvent = event => {
  const star = parseInt(event.target.dataset.star)

  switch (event.type) {
    case "click":
      if (star) {
        rating.value = star
        currentHoverRating.value = star
      }
      break
    case "mousemove":
      if (star) {
        currentHoverRating.value = star
      }
      break
    case "mouseleave":
      currentHoverRating.value = rating.value
      break
  }
}

const closeModal = () => {
  isVisible.value = false
  emits("update:showModal", false)
}

const submitFeedback = () => {
  console.log("Rating:", rating.value)
  console.log("Feedback:", feedback.value)
  closeModal()
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}

.modal {
  background: #fff;
  padding: 20px;
  border-radius: 10px;
  width: 400px;
  max-width: 90%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.close-button {
  background: none;
  border: none;
  font-size: 1.5rem;
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

.star-rating {
  display: flex;
  cursor: pointer;
  margin-bottom: 20px;
}

.star {
  font-size: 2rem;
  color: #ccc;
}

.star--filled {
  color: gold;
}

textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 20px;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  background: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background: #0056b3;
}
</style>
