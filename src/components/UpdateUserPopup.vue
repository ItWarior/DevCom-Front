<template>
  <div class="updateUserPopup" v-bind:class="{hidden: !flagUpdateUserPop}">
    <form @submit.prevent="onSubmit">
      <input minlength="4" required type="text" name="login" v-model="userToUpdate.login">
      <input minlength="2" required type="text" name="firstName" v-model="userToUpdate.firstName">
      <input minlength="2" required type="text" name="lastName" v-model="userToUpdate.lastName">
      <input disabled type="text" name="email" v-model="userToUpdate.email">
    </form>
    <div class="buttonContainer">
      <button @click="$emit('cancelUpdatePopup')">cancel</button>
      <button @click="onSubmit">save</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  methods: {
    onSubmit() {
      const dataUser = {
        login: this.userToUpdate.login,
        firstName: this.userToUpdate.firstName,
        lastName: this.userToUpdate.lastName,
      }

      axios
          .put(`http://localhost:3000/user/${this.userToUpdate.email}`, dataUser)
          .then((response) => {
            this.$emit('userCallback', response.data)
            this.$emit('cancelUpdatePopup')
          })
          .catch((error) => {
            alert(`localhost error: ${error.response.data.message}`);
          });
    }
  },
  props: {
    flagUpdateUserPop: {
      type: Boolean
    },
    userToUpdate: {
      Object
    }
  }
}
</script>

<style scoped>
.updateUserPopup {
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
