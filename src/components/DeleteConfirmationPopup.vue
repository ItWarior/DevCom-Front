<template>
  <div class="container">
    <div class="deleteContainer" v-bind:class="{hidden: !flagDeleteUserPop}">
      <span>Are you sure?</span>
      <div class="buttonContainer">
        <button @click="deleteConfirmation(false)">No</button>
        <button @click="deleteConfirmation(true)">Yes</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    flagDeleteUserPop: Boolean
  },
  methods: {
    deleteConfirmation(answer) {
      this.$emit('submit', answer)
    }
  }
}
</script>

<style scoped>
.container{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.deleteContainer {
  display: flex;
  justify-content: center;
  flex-direction: column;
  background: linear-gradient(0deg, rgba(35, 201, 134, 0.79) 0%, rgba(45, 117, 253, 0.87) 100%);
  height: 200px;
  width: 300px;
  border-radius: 5px;
}

span {
  height: 50%;
}

.buttonContainer {
  display: flex;
  justify-content: space-around;
  position: relative;
  bottom: 10px;
}

.hidden {
  visibility: hidden;
  display: none;
}
</style>
