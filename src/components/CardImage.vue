<script>

export default {
  name: "CardImage",
  props: [
    'image'
  ],
  computed: {
    backgroundImage() {
      return 'url(' + (this.image.smallImageUrl ?? this.image.originalImageUrl) + ')'
    }
  },
  methods: {
    openOriginal() {
      window.open(this.image.originalImageUrl)
    }
  }
}
</script>

<template>
  <div class="card" @click="openOriginal">
    <p class="card__text">{{ image.name }}</p>
  </div>
</template>

<style scoped lang="scss">
.card {
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  width: 275px;
  height: 275px;
  background-image: v-bind('backgroundImage');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  border-radius: 12px;
  box-shadow: inset 0 0 rgba(0, 0, 0, 1);
  transition: box-shadow 0.3s ease-in-out 0s;
}

.card:hover {
  box-shadow: inset 0 -20px 40px 0 rgba(0, 0, 0, 1);
}

.card__text {
  width: 260px;
  margin-bottom: 5px;
  text-align: center;
  overflow: hidden;
  text-overflow: ellipsis;
  opacity: 0;
  align-self: flex-end;
  transition: opacity 0.33s linear;
}

.card:hover {
  .card__text {
    opacity: 1;
    align-self: flex-end;
  }
}

</style>