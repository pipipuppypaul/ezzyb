<template>
  <div class="first-page">
    <h2>Enter Flow ID</h2>
    <form @submit.prevent="onSubmit">
      <div class="form-group">
        <label for="fid">FlowID:</label>
        <input type="text" id="fid" v-model="fID" required>
      </div>
      <div class="form-actions">
        <button type="submit">Next</button>
      </div>
    </form>
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      fID: ''
    }
  },
  methods: {
    onSubmit() {
      const formData = {
        flowID: this.fID
      };
      this.$emit('next', formData);
    }
  }
}
</script>
  
<style scoped>
/* Add styling as needed */
</style>