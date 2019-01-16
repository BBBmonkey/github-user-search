<template>
    <app-user-table></app-user-table>
</template>

<script>
  import userTable from './components/userTable.vue'

export default {
  data () {
    return {

    }
  },

  components: {
    appUserTable: userTable
  }
}
</script>

<style lang="scss">
  body {
    padding: 30px;
  }
</style>
