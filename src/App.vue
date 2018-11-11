<template>
  <v-app dark>
    <v-content >
      <Films/>
    </v-content>
  </v-app>
</template>

<script>
import Films from './components/Films'

export default {
  name: 'App',
  components: {
    Films
  },
  data () {
    return {
      //
    }
  }
}
</script>
