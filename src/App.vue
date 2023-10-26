<template>
  <SubscribeForm />
</template>

<script>
import SubscribeForm from './components/SubscribeForm';

export default {
  name: 'App',
  components: { SubscribeForm}
}
</script>

<style>
  @import url(https://fonts.googleapis.com/css?family=Open+Sans:300, 400, 700);
@import url(https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css);

* {
  box-sizing: border-box;
}

body {
  background-image: linear-gradient(to right, #034378, #2d4e68);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  font-family: "Open Sans", sans-serif;
}

a {
  text-decoration: none;
}

h1 {
  font-family: "Open Sans", sans-serif;
  font-weight: 300;
}

.row {
  max-width: 900px;
  margin: 50px auto 0;
}

</style>
