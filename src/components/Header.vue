<template>
  <div>
    <ul class="header-list">
      <li class="header-list-item">Fancy Quiz App</li>
      <li class="header-list-item">Counter: {{ numCorrect }}/{{ numTotal }}</li>
    </ul>
  </div>
</template>
<script>
export default {
  props: {
    numCorrect: Number,
    numTotal: Number,
  },
};
</script>
<style scoped>
.header-list {
  display: flex;
  border-bottom: 1px solid grey;
  list-style: none;
}

.header-list-item {
  margin-right: 1em;
  padding: 1em;
}
</style>
