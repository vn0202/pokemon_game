<template>
  <div class="screen-result">
    <div class="screen-content">
      <img
        src="../assets/images/9.jpg"
        alt=""
        class="img-user"
        style="object-fit: contain"
      />
      <h1>✨ Congratulations ✨</h1>
      <h3>✨ You are my sunshine ✨</h3>

      <p>
        Your result is :
        <span class="score">{{ Math.round(result / 1000) }}s</span>
      </p>
      <button @click="restart">Restart</button>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    result: {
      type: [String, Number, Object],
      require: true,
    },
  },
  methods: {
    restart() {
      this.$emit("restart");
    },
  },
};
</script>
<style lang="css" scoped>
.screen-result {
  width: 100%;
  height: 100vh;
  margin: 0 auto;
  display: flex;
}
.screen-content {
  margin: auto;
  text-align: center;
}
.screen-content .score {
  color: green;
}
.img-user {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 100%;
  box-shadow: 1px 4px 4px #ee2ca0;
}
</style>
