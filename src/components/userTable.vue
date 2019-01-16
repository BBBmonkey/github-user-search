<template>
  <div>
    <input type="text" v-model="userToSearch">
    <button class="btn btn-success" @click="searchUser"> Search </button>
    <transition name="slide" mode="out-in">
      <table class="table">
        <thead class="thead-dark">
        <tr>
          <th scope="col">ID</th>
          <th scope="col" class="clickable" @click="sortByLogin">login</th>
          <th scope="col">Avatar</th>
          <th scope="col">#ofRepos</th>
        </tr>
        </thead>
        <tbody>
          <user-table-row v-for="user in users" :user="user"></user-table-row>
        </tbody>
      </table>
    </transition>
  </div>
</template>

<script>
  import userTableRow from './userTableRow'

  export default {
    components: {userTableRow},

    data(){
      return {
        userToSearch: "",
        users: [],
      }
    },

    computed: {
      searchString() {
        return this.userToSearch + `\+in\%3Afullname`;
      }
    },

    methods: {
      searchUser(){
        this.$http.get(`https://api.github.com/search/users?q=${this.searchString}&type=Users`)
          .then( result => {
            this.users = result.body.items;
          })
      },

      sortByLogin(){
        this.users.sort(function(a, b) {
          let nameA = a.login.toUpperCase(); // ignore upper and lowercase
          let nameB = b.login.toUpperCase(); // ignore upper and lowercase
          if (nameA < nameB) {
            return -1;
          }
          if (nameA > nameB) {
            return 1;
          }

          return 0;
        });
      }
    }
  }
</script>

<style scoped>
  .clickable{
    cursor: pointer;
  }

  .slide-enter-active {
    animation: slide-in 200ms ease-out forwards
  }

  .slide-leave-active {
    animation: slide-out 200ms ease-out forwards
  }

  @keyframes slide-in {
    from {
      transform: translateY(-30px);
      opacity: 0;
    }

    to {
      transform: translateY(0);
      opacity: 1;
    }
  }

  @keyframes slide-out {
    from {
      transform: translateY(0);
      opacity: 1;
    }

    to {
      transform: translateY(-30px);
      opacity: 0;
    }
  }
</style>
