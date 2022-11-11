<template>
  <div class="container">
    <client-register></client-register>
  </div>
</template>
<script>
import ClientRegister from '../components/ClientRegister.vue';
export default{
  name: 'Register-View',

  components: {
    ClientRegister
  }
}
</script>