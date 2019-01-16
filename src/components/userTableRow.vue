<template>
  <tr >
    <td> {{user.id}}</td>
    <td> {{user.login}}</td>
    <td> <img :src="user.avatar_url" class="avatar"></td>
    <td> {{numOfRepos}}</td>
  </tr>
</template>

<script>
  export default {
    props:['user'],

    data(){
      return {
        numOfRepos: '0'
      }
    },

    created(){
      this.$http.get(this.user.repos_url).then(repos => {
        this.numOfRepos = repos.length;
      })
    },
  }
</script>

<style scoped>
  .avatar {
    width: 50px;
    height: 50px;
  }
</style>
