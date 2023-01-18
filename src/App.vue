<template>
  <UserTable
      v-bind:users="users"
      v-on:userToUpdate="updateUser"
      v-on:userToDelete="deleteUser"
  />
  <button class="createUserBtn" v-on:click="changeShowCreatePopup">Create New User</button>
  <CreateUserPopup
      v-if="flagCreateUserPop"
      v-on:cancelCreatePopup="changeShowCreatePopup"
      v-bind:showPopup="flagCreateUserPop"
      v-on:addUser="addNewUser"
  />
  <UpdateUserPopup
      v-if="flagUpdateUserPop"
      v-on:cancelUpdatePopup="changeShowUpdatePopup"
      v-bind:flagUpdateUserPop="flagUpdateUserPop"
      v-on:updateUser="updateUser"
      v-on:userCallback="updatedUser"
      v-bind:userToUpdate="infoToUpdateUser"

  />
  <DeleteConfirmationPopup
      v-if="flagDeleteUserPop"
      v-bind:flagDeleteUserPop="flagDeleteUserPop"
      v-on:submit="deleteConfirmationPopup"
  />

</template>

<script>
import axios from "axios";
import UserTable from '@/components/UserTable';
import CreateUserPopup from '@/components/CreateUserPopup';
import UpdateUserPopup from "@/components/UpdateUserPopup";
import DeleteConfirmationPopup from "@/components/DeleteConfirmationPopup";

export default {
  name: 'App',
  components: {
    DeleteConfirmationPopup,
    UserTable,
    UpdateUserPopup,
    CreateUserPopup,
  },
  methods: {
    changeShowCreatePopup() {
      this.flagCreateUserPop = !this.flagCreateUserPop
    },
    changeShowUpdatePopup() {
      this.flagUpdateUserPop = !this.flagUpdateUserPop
    },

    changeShowDeletePopup() {
      this.flagDeleteUserPop = !this.flagDeleteUserPop
    },

    addNewUser(user) {
      this.users.push(user)
    },

    deleteConfirmationPopup(answer) {
      if (answer) {
        axios
            .delete(`http://localhost:3000/user/${this.emailUserToDelete}`)
            .then((response) => {
              this.users = this.users.filter(user => user.id !== response.data.id)
            })
            .catch((error) => {
              alert(`localhost error: ${error.message}`);
            });
      }
      this.emailUserToDelete = ''
      this.changeShowDeletePopup()
    },

    updateUser(user) {
      this.infoToUpdateUser = {...user}
      this.changeShowUpdatePopup();

    },
    updatedUser(user) {
      this.users[this.users.findIndex(i => user.id === i.id)] = user

    },
    deleteUser(email) {
      this.emailUserToDelete = email
      this.changeShowDeletePopup();
    }
  },
  data() {
    return {
      flagCreateUserPop: false,
      flagDeleteUserPop: false,
      flagUpdateUserPop: false,
      emailUserToDelete: '',
      infoToUpdateUser: '',
      users: []
    }
  },
  mounted() {
    axios
        .get("http://localhost:3000/user")
        .then((response) => {
          this.users = response.data
        })
        .catch((error) => {
          alert(`localhost error: ${error.message}`);
        });
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.createUserBtn{
  position: absolute;
  left: 10px;
  margin-top: 10px;
}
</style>
