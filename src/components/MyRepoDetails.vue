<!-- src/components/MyRepoDetails.vue -->
<template>
  <div class="repo-details-container">
    <div v-if="loading" class="loader">Loading details...</div>
    <div v-else>
      <h2>{{ details.name }}</h2>
      <p>{{ details.description }}</p>
      <!-- Add more details as needed -->
    </div>
  </div>
</template>

<script>
export default {
  props: {
    details: Object,
    loading: Boolean,
  },
};
</script>

<style scoped>
.repo-details-container {
  height: 100%;
  width: 25%;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: rgb(77, 248, 9);
}

.details-row {
  height: 20px;
  width: 70%;
  display: flex;
  margin-top: 25px;
  align-items: center;
}

.label {
  font-weight: bold;
  margin-right: 20px;
  color: #696c71;
}

.value {
  color: rgb(77, 248, 9);
}

.loader {
  color: rgb(77, 248, 9);
  margin-left: 5%;
}
</style>
