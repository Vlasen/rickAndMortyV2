<script setup>
defineProps(['locations'])
const emit = defineEmits(["mouseMoveGradient", "mouseLeave"])

const mouseMoveGradient = (event) => {
  emit("mouseMoveGradient", event);
};
const mouseLeave = (event) => {
  emit("mouseLeave", event);
};
</script>

<template>
  <div class="locationsList">
    <div class="locationsCard"
      v-for="location in locations" :key="location.id">

      <div class="mainLocationsInfo">
        <div class="info">
          <div class="topGradientLayer" @mousemove="mouseMoveGradient" @mouseleave="mouseLeave"></div>
          <div class="nameCharacter"> {{ character.name }} </div>

          <label>{{ character.status }}</label>
          <div class="species"> {{ character.species }} </div>
        </div>
      </div>
    </div>
  </div>
</template>