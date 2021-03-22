<template>
  <div class="result">
    <h1>{{ result }}</h1>
  </div>
</template>

<script>
export default {
  name: "Results",
  props: ["result"],
};
</script>

<style lang="scss" scoped>
.result {
  text-align: center;
  padding: 32px;
}
</style>
