<template>
  <div id="app">
    <Form />
  </div>
</template>

<script>
import Form from "@/components/Form";
export default {
  name: "App",
  components: { Form },
};
</script>

<style lang="scss">
#app {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  margin: 40px auto;
  max-width: 700px;
}
</style>
