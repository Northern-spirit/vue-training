<template>
  <div>
    <h1>Тикет</h1>
    <div class="wrapper">
      <input
        v-model="textInput"
        class="input-style"
        type="text"
        placeholder="add text"
        @keydown.enter="addCard"
        @emitAddCard="addCard"
      />
      <button class="button-style" type="button" @click="addCard">Push</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "InputPage",
  data() {
    return {
      textInput: "",
    };
  },
  methods: {
    addCard() {
      alert(this.textInput);
    },
  },
};
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
}

.input-style {
  width: 150px;
  margin-bottom: 10px;
}

.button-style {
  width: 100px;
  height: 20px;
  border-radius: 25px;
  margin-left: 20px;
}
</style>