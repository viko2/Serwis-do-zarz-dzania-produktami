<template lang="">
    <header class="header">
      <v-container class="header__container">
      <img src="https://www.svgrepo.com/show/48219/avocado.svg" width="35" height="35" class="header__logo"/>
        <div class="">  
          <h2 class="header__name">Product app</h2>
        </div>
        <div v-if="user" class="header__login">
          <h2 class="header__name">{{user.name}}</h2>  
          <img width="35" height="35" :src="user.avatar" />
          <p class="header__login--logout" @click="logout()">Wyloguj się</p>
        </div>
        <div v-else @click="login()" class="header__login">
          <p class="header__login--login">Login</p>
        </div>
       </v-container> 
    </header>
</template>
<script>
import { computed, ref, watch } from '@vue/runtime-core'
import { useRouter } from 'vue-router';

export default {
    name: 'HeaderComponent',
    setup () {
    watch(localStorage, console.log(1))  
    const user = ref(JSON.parse(localStorage.getItem('user')))
    const router = useRouter()
    const logout = () => {
      localStorage.removeItem('user')
      user.value = null
      router.push('/')
    }
    const login = () => {
      router.push('/')
    }
    return {
      user,
      login,
      logout,
    }
  },
}

</script>
<style lang="scss" scoped>
  .header {
    width: 100%;
    height: 50px;
    background-color: #dff0ff;
    &__container {
      display: flex;
      height: 50px;
      align-content: center;
      align-items: center;
      justify-content: space-between;
    } 
    &__logo {
      padding-left: 10px;
    }
    &__name {
      @media (max-width: 767px) {
        display: none;
      }
    }
    &__login {
      display: flex;
      flex-direction: row;
      width: 250px;
      align-content: center;
      align-items: center;
      justify-content: space-between;
      @media (max-width: 767px) {
          font-size: 16px;
          width: 150px;
        }
      &--logout {
        font-size: 18px;
        color: red;
        margin-right: 15px;
        cursor: pointer;
        @media (max-width: 767px) {
          font-size: 16px;
        }
      }
      &--login {
        font-size: 18px;
        color: green;
        margin-right: 15px;
        cursor: pointer;
      }
    }
  }
</style>