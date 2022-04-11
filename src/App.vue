<template>
  <InputPage />
  <CardPage />
</template>

<script>
import InputPage from "./components/InputPage.vue";
import CardPage from "./components/CardPage.vue";

export default {
  data() {
    return {};
  },
  name: "App",
  components: {
    InputPage,
    CardPage,
  },
  methods: {},
};
</script>

<style>
</style>
