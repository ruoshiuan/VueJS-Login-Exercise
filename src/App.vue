<template>
  <LoginForm slogan="VueJS Login Exercise" />
</template>

<script>
import LoginForm from "./components/LoginForm.vue";
export default {
  name: "App",
  components: {
    LoginForm,
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: #2c3e50;
  color: #fff;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
