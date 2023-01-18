<template>
  <div>
    <table aria-colspan="7">
      <tr style="font-weight: bold">
        <th>ID</th>
        <th>Login</th>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Email</th>
        <th></th>
        <th></th>
      </tr>
      <UserField
        v-for="(user, i) of users"
        v-bind:index="i"
        v-bind:user="user"
        v-on:userToUpdate="updateUser"
        v-on:userToDelete="deleteUser"
      />
    </table>
  </div>
</template>

<script>
import UserField from '@/components/UserField'
  export default {
    components:{
      UserField
    },
    props: ['users'],
    methods:{
      updateUser(user){
        this.$emit("userToUpdate", user)
      },
      deleteUser(userEmail){
        this.$emit("userToDelete", userEmail)
      }
    }
  }
</script>

<style scoped>
table{
  width: 100%;
  border: 1px solid red;
  border-collapse: collapse;
}
th{
  border: 1px solid #0011ff;
}
td{
  text-align: left;
}



</style>
