<template>
  <div class="createUserPopup" v-bind:class="{hidden: !showPopup}">
    <form @submit.prevent="onSubmit">
      <input minlength="4" required type="text" name="login" placeholder="Login" v-model="login">
      <input minlength="2" required type="text" name="firstName" placeholder="First Name" v-model="firstName">
      <input minlength="2" required type="text" name="lastName" placeholder="Last Name" v-model="lastName">
      <input :class="{ invalidInput: isValidEmail }" v-on:keydown="regex = false" required type="text" name="email"
             placeholder="Email" v-model="email">
    </form>
    <div class="buttonContainer">
      <button @click="$emit('cancelCreatePopup')">cancel</button>
      <button @click="onSubmit">save</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  computed: {
    isValidEmail() {
      const regexEmail = /[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/;
      return !regexEmail.test(this.email)
    }
  },
  methods: {
    onSubmit() {
      const regexEmail = /[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/;

      if (regexEmail.test(this.email)) {
        const dataUser = {
          login: this.login,
          firstName: this.firstName,
          lastName: this.lastName,
          email: this.email
        }

        axios
            .post("http://localhost:3000/user", dataUser)
            .then((response) => {
              this.$emit('addUser', response.data)
              this.$emit('cancelCreatePopup')
            })
            .catch((error) => {
              alert(`localhost error: ${error.response.data.message}`);
            });

      }
    }
  },
  data() {
    return {
      login: '',
      firstName: '',
      lastName: '',
      email: ''
    }
  },
  props: {
    showPopup: {
      type: Boolean
    }
  }
}
</script>

<style scoped>
.createUserPopup {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 50%;
}

form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 10px 10px 40px 10px;
  border: 1px solid black;
  border-radius: 10px;
  width: 200px;
  background: linear-gradient(0deg, rgba(34, 195, 130, 0.788666540835084) 0%, rgba(45, 117, 253, 0.8558934315913865) 100%);
  box-shadow: 3px 4px 5px #6b3e13;
}

input {
  width: 70%;
  margin: 10px;
  border-radius: 5px;
}

.invalidInput {
  color: red;
}

.buttonContainer {
  display: flex;
  justify-content: space-between;
  width: 200px;
  margin: 10px;
  position: absolute;
  bottom: 5px;
}

button {
  width: 80px;
  border-radius: 10px;
  margin: 0 10px;
}

.hidden {
  visibility: hidden;
  display: none;
}
</style>
