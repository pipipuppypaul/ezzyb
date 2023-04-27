<template>
  <div class="second-page">
    <h2>Flow details</h2>
    <form @submit.prevent="onSubmit">
      <div class="form-group">
        <label for="tp">Flow Type:</label>
        <input class="input" type="text" id="tp" v-model="fType" required>
      </div>
      <div class="form-group">
        <label for="act">Flow Action:</label>
        <input class="input" type="text" id="act" v-model="fAction" required>
      </div>
      <div class="form-actions">
        <button class="button" type="submit">Submit</button>
        <button class="button" type="button" @click="goBack"> Back </button>
        <button class="cancel" @click="cancel">Cancel</button>
      </div>
    </form>
  </div>
</template>
  
<script>
export default {
  props: {
    data: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      fType: '',
      fAction: ''
    }
  },
  methods: {
    onSubmit() {
      const formData = {
        flowType: this.fType,
        flowAction: this.fAction
      };
      this.$emit('submit', formData);
    },
    goBack() {
      this.$emit('prev');
    },
    cancel() {
      this.$emit('cancel');
    }
  }
}
</script>
  
<style scoped>
/* Add styling as needed */
</style>
  