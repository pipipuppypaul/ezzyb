<template>
  <div id="app">
    <button class="button" @click="showPopup">Add WorkFlow</button>

    <div class="popup-container" v-if="show">
      <div class="popup" v-if="showPopup">
        <FirstPage v-if="showFirstPage" @next="onFirstPageNext" @cancel="closePopup"></FirstPage>
        <SecondPage v-if="showSecondPage" :data="formData" @submit="onSecondPageSubmit" @prev="prevPage"
          @cancel="closePopup"></SecondPage>
      </div>

    </div>

  </div>
</template>
  
<style>
.popup-container {
  position: fixed;
  z-index: 1000;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup {
  background-color: #fff;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  max-width: 500px;
  width: 100%;
  max-height: 80%;
  overflow: auto;
}

.button {
  margin-left: 10px;
  padding: 8px 16px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 4px;
  border: none;
  background-color: #3f51b5;
  color: #fff;
  cursor: pointer;
}

.cancel {
  margin-left: 10px;
  padding: 8px 16px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 4px;
  border: none;
  background-color: #3f51b5;
  color: #fff;
  cursor: pointer;
  float: right;
}

.form-group {
  margin-bottom: 20px;
}

.label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.input {
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 10px;
  width: 100%;
}

.button-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
}

.button:hover {
  background-color: #283593;
}

button:focus {
  outline: none;
}

@media (max-width: 500px) {
  .popup {
    max-width: 100%;
    max-height: 100%;
  }
}
</style>



<script>
import FirstPage from './components/FirstPage.vue';
import SecondPage from './components/SecondPage.vue';
import axios from 'axios'

export default {
  components: {
    // TODO: if you have more pages in the future,
    // you should use page numbers instead of page names
    FirstPage,
    SecondPage
  },
  data() {
    return {
      show: false,
      showFirstPage: false,
      showSecondPage: false,
      formData: {}
    }
  },
  methods: {
    showPopup() {
      this.showFirstPage = true,
        this.show = true
    },
    closePopup() {
      this.show = false
      this.formData = {}
      this.showFirstPage = false
      this.showSecondPage = false
    },
    onFirstPageNext(data) {
      // this.formData = { ...this.formData, ...data };
      this.formData = Object.assign({}, this.formData, data)
      this.showFirstPage = false;
      this.showSecondPage = true;
    },
    prevPage(data) {
      this.page -= 1
      this.showFirstPage = true;
      this.showSecondPage = false;
      this.formData = { ...this.formData, ...data };
    },
    onSecondPageSubmit(data) {
      // this.formData = { ...this.formData, ...data };
      this.formData = Object.assign({}, this.formData, data)
      console.log(this.formData); // Log the final form data
      // you can post data to your backend here:
      axios.post('/api/save-data', data)
        .then(response => {
          console.log(response.data)
        })
        .catch(error => {
          console.log(error.response.data)
        })
      this.closePopup();
    }
  }
}
</script>
  