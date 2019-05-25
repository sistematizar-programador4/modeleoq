<template>
  <v-app>
    <EOQ/>
  </v-app>
</template>

<script>
import EOQ from './components/EOQ'
import Table from './components/Table'

export default {
  name: 'App',
  components: {    
    Table,
    EOQ
  },
  data () {
    return {
      //
    }
  }
}
</script>