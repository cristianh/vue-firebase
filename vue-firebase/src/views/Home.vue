<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <DataBaseFire></DataBaseFire>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import DataBaseFire from '@/components/DataBaseFire.vue'

export default {
  name: 'home',
  components: {
    HelloWorld,
    DataBaseFire
  }
}
</script>
