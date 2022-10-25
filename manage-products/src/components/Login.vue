<template>
    <div class="login">
      <v-form class="login__form" v-model="valid">
        <img src="https://www.svgrepo.com/show/48219/avocado.svg" width="80" height="80" class="login__logo"/>
        <h2 class="login__header">Załoguj się</h2>
          <v-text-field
            v-model="email"
            :rules="emailRules"
            label="E-mail"
            required
          ></v-text-field>
          <v-text-field
            v-model="password"
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[rules.required, rules.min]"
            :type="show1 ? 'text' : 'password'"
            name="input-10-1"
            label="Type your password"
            hint="At least 8 characters"
            @click:append="show1 = !show1"
          ></v-text-field>
          <v-text-field
            v-model="name"
            :rules="textRules"
            label="Name"
            required
          ></v-text-field>
          <div class="login__error">
            <p v-show="showError" class="login__error--text">Nieprawidłowy login lub hasło lub imie</p>
          </div>
          <v-btn
            class="login__form--button"
            color="secondary"
            :disabled="!valid"
            @click="login()"
            elevation="2"
          >Login</v-btn>
      </v-form>
    </div>
      
</template>

<script>
import { ref } from '@vue/reactivity'
import { useRouter } from 'vue-router';
import axios from 'axios'
import { onMounted } from '@vue/runtime-core';

export default {
    name: 'LoginPage',
    setup () {
      const valid = ref(false)
      const email = ref('');
      const password = ref('');
      const name = ref('');
      const showError = ref(false)
      const show1 = ref(false);
      const router = useRouter()
      const rules = {
        required: value => !!value || 'Required.',
        min: v => v.length >= 8 || 'Min 8 characters',
        emailMatch: () => (`The email and password you entered don't match`),
      };
      const textRules = [
        value => !!value || 'Required.',
        value => (value && value.length >= 3) || 'Min 3 characters',
        value => (value && value.length <= 64) || 'Max 64 characters',
      ];
      console.log(router);
      const emailRules = [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ];
      const login = async () => {
        let result = await axios.get('https://api.escuelajs.co/api/v1/users')
        let filteredUser = result.data.find(x => x.email === email.value && x.password === password.value && x.name === name.value)
        
        if (result.status == 200 && filteredUser) {
          localStorage.setItem("user", JSON.stringify(filteredUser))
          router.push('/products')
        } else {
          showError.value = true
          setTimeout(() => showError.value = false, 3000);
        }
      };
      onMounted(() => {
        let user = localStorage.getItem('user')
        if (user) {
          router.push('/products')
        }
      });
      return {
        email,
        login,
        valid,
        showError,
        password,
        name,
        textRules,
        emailRules,
        show1,
        rules,
      }
  },
}

</script>
<style lang="scss" scoped>
  .login {
    background-image: url(https://images.wallpaperscraft.com/image/single/stars_space_milky_way_381446_1920x1080.jpg);
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    &__error {
      position: relative;
      display: flex;
      height: 30px;
      padding: 20px;
      &--text {
        color: red;
        position: absolute;
        text-align: center;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
      }
    }
    &__form {
      width: 25%;
      padding: 25px;
      background-color: white;
      @media (max-width: 1366px) {
         width: 45%; 
      }
      @media (max-width: 767px) {
         width: 75%; 
      }
      &--button {
        display: flex;
        margin: auto;
      }
    }
    &__logo {
      display: flex;
      align-items: center;
      margin-left: auto;
      margin-right: auto;
    }
    &__header {
      text-align: center;
      padding: 20px;
    }
  }
</style>