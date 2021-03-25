<template>
  <div class="wrapper">
    <h2>📈</h2>
    <transition name="loader-transition">
      <p>
        Loading the current <br />
        stock price...
      </p>
    </transition>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
.wrapper {
  margin-top: 80px;
  border-radius: 16px;
  text-align: center;
  padding: 40px;
  margin: 0 auto;
  width: 300px;
}

.loader-transition-enter-active {
  transition: all 0.3s ease;
}
.loader-transition-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.loader-transition-enter, .loader-transition-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>
