<template>
    <div class="second-page">
        <h2>Flow details</h2>
        <form @submit.prevent="onSubmit">
            <div class="form-group">
                <label for="tp">Flow Type:</label>
                <input type="text" id="tp" v-model="fType" required>
            </div>
            <div class="form-group">
                <label for="act">Flow Action:</label>
                <input type="text" id="act" v-model="fAction" required>
            </div>
            <div class="form-actions">
                <button type="submit">Submit</button>
                <button type="button" @click="goBack">Back</button>
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
        }
    }
}
</script>
  
<style scoped>
/* Add styling as needed */
</style>
  