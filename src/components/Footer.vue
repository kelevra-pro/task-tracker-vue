<template>
  <footer>
    <p>Copyright &copy; 2022</p>
    <router-link to="/about">About</router-link>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
};
</script>

<style scoped>
footer {
  margin-top: 30px;
  text-align: center;
}

a {
  color: #333;
}
</style>
