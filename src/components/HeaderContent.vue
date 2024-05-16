<script>
export default {
  name: "HeaderContent"
}
</script>

<template>
  <header class="header">
    <div class="header__card">
      <h1 class="header__card-title">Image Host Test</h1>
      <p class="header__card-text">
        Прототип хостинга изображений. Ну, вы меня пон.
      </p>
    </div>
  </header>
</template>

<style scoped lang="scss">
.header {
  display: flex;
  justify-content: center;
}

.header__card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-top: 150px;
  border: aquamarine;
  text-align: center;
}

.header__card-title {
  font-size: 72px;
  font-weight: 700;
  background: radial-gradient(circle, #69DB8B, #53D8B9);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.header__card-text {
  font-size: 24px;
  margin-top: 12px;
}
</style>