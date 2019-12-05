<template>
  <div class="home">
    <!--img alt="Vue logo" src="../assets/logo.png"-->
    <Main msg="零時政府專案列表" />
  </div>
</template>

<script>
// @ is an alias to /src
import Main from '@/components/Main.vue'

export default {
  name: 'home',
  components: {
    Main
  }
}
</script>
