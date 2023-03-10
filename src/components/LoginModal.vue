<template>
  <ModalBackground>
    <div class="login-modal">
      <div class="login-functions">
        <button @click="setLoginFunction(true)" :class="loggingUserIn ? 'btn-active' : null">
          <img alt="Login" :src="assetspath('./ui/person.webp')" />
          <span>Login</span>
        </button>
        <button @click="setLoginFunction(false)" :class="!loggingUserIn ? 'btn-active' : null">
          <img alt="Register" :src="assetspath('./ui/person_add.webp')" />
          <span>Register</span>
        </button>
      </div>
      <CloseButton @click="$emit('closeLoginModal')" />
      <form>
          <label for="email">E-Mail:</label><br/>
          <input v-model="email" type="email" id="email" name="email" />
          <label for="password">Password:</label><br/>
          <input v-model="password" type="password" id="password" name="password" />
      </form>
      <button @click="loginUser" id="submit">
        <span v-if="loggingUserIn">Log In</span>
        <span v-if="!loggingUserIn">Sign Up</span>
      </button>
      <strong v-if="error" id="errorMessage">{{ errorMessage }}</strong>
    </div>
  </ModalBackground>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { fetchData } from '@/mixins/fetchData'
import { fetchImages } from '@/mixins/fetchImages'
import ModalBackground from '@/components/ModalBackground.vue'
import CloseButton from '@/components/buttons/CloseButton.vue'
import axios from 'axios'
import Cookies from 'js-cookie'

export default defineComponent({
  components: {
    ModalBackground,
    CloseButton
  },
  props: {
    loginModalOpen: Boolean
  },
  mixins: [fetchData, fetchImages],
  data () {
    const email = ''
    const password = ''
    const errorMessage = ''

    return {
      email,
      password,
      errorMessage,
      loggingUserIn: true,
      error: false
    }
  },
  methods: {
    async loginUser () {
      await axios.post('http://localhost:1337/api/auth/local', {
        identifier: this.email,
        password: this.password
      }).then(response => {
        this.error = false
        Cookies.set('token', response.data.jwt)
        this.$emit('closeLoginModal')
        this.$emit('logUserIn')
      }).catch(error => {
        this.error = true
        console.log(error.response)
        this.errorMessage = 'The account details entered are incorrect'
      })
    },
    setLoginFunction (state) {
      this.loggingUserIn = state
    },
    setError () {
      this.error = true
    }
  }
})
</script>

<style scoped lang="scss">
  .login-modal {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 80vw;
      height: 80vh;
      background-color: rgb(255, 255, 255);

      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap: 10px;
      padding: 10px;

      .login-functions {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background-color: var(--button-bg-color);
        position: relative;
        padding: 10px;

        button {
          background: none;
          border: none;
          color: white;
          font-size: 1.25rem;

          display: grid;
          grid-template-columns: 1fr 1fr;
          grid-template-rows: 1fr;
          align-items: center;
          padding: 10px;
          width: 100%;

          img {
            height: 20px;
            filter: invert(1);
          }
        }

        button:hover {
          background: var(--button-bg-color-hover);
        }

        .btn-active {
          background: var(--button-bg-color-hover);
        }
      }

      form {
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
      }

      #submit {
          width: 100%;
          background-color: var(--button-bg-color);
      }

      #errorMessage {
        font-size: 1rem;
        text-align: center;
        color: var(--error-color);
      }
  }
</style>
