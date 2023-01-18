<template>
  <tr>
    <td>{{ index + 1 }}</td>
    <td>{{ user.login }}</td>
    <td>{{ user.firstName }}</td>
    <td>{{ user.lastName }}</td>
    <td>{{ user.email }}</td>
    <td>
      <button @click="updateUser">Edit</button>
    </td>
    <td>
      <button @click="deleteUser">Delete</button>
    </td>
  </tr>
</template>

<script>

export default {

  props: {
    user: {
      type: Object,
      required: true
    },
    index: Number
  },

  methods: {
    updateUser(){
      this.$emit("userToUpdate", this.user)
    },
    deleteUser(){
      this.$emit("userToDelete", this.user.email);
    }
  }
}
</script>

<style scoped>
td {
  text-align: left;
}
</style>
