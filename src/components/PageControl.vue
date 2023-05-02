<template>
  <div class="page-container">
    <div>
      <div id="page_start" v-if="currentPageNumber == 0">
        <span style="color: #303133">Let's setup your account!</span>
        <button class="next" @click="startPage">start</button>
      </div>
      <div id="page_1" v-if="currentPageNumber == 1">
        <h3>Step1. Your use cases: </h3>
        <h4>Please tick all the cases that is applicable to you</h4>
        <input type="checkbox" v-model="checkedCases.EEC" value=true> Employee Expense Claims <br>  
        <!-- Team Lead, CFO, CEO... -->
        <input type="checkbox" v-model="checkedCases.CCCE" value=true> Company Credit Card Expenses <br>  
        <!-- Employee Expense on Credit Card -->
        <input type="checkbox" v-model="checkedCases.AB" value=true> Approve Bills <br>  
        <!-- Approval by Department -->
        <div class="form-actions">
          <button class="next" @click="nextPage" >Next</button>
          <button class="cancel" @click="cancelPage">Cancel</button>
        </div>
      </div>
      <div id="page_2" v-if="currentPageNumber == 2">
        <h3>Step2. Employee expense claims</h3>
        <div >
          <p>1. Department Approver config:</p>
          <p id="task-counter">Approves added: {{ taskArr.length }}</p>
          <div class="add-task-wrapper">
            <button id="add-task-btn" v-on:click="addItem()" :disabled=!checkedCases.EEC>+ Add Approver</button>
          </div>
          <div class="task-list" v-for="(task, idx) in taskArr" :key="idx">
            <div class="list-item">
              <div class="container">
                ({{ idx + 1 }}) {{ task.name }}  
                <button class="delete-item btn" v-on:click="deleteItem(idx)">
                  X
                </button> <br> 
                name: <input  type="text" id="name" v-model="formData.dptmt_appr_names[idx]" >  <br> 
                email: <input  type="text" id="email" v-model="formData.dptmt_appr_emails[idx]" >  <br>
              </div>
            </div>
          </div>
        </div>
        <div class="form-actions">
          <button class="cancel" @click="cancelPage">Cancel</button>
          <button class="prev" @click="prevPage" >Previous</button>
          <button class="next" type="submit" @click="nextPage" >Next</button>
        </div>
      </div>

      <div id="page_3" v-if="currentPageNumber == 3">
        <h3>Step2. Employee expense claims</h3>
        <div>
          <p>2. CFO & CEO Approvers config:</p>
          Default Approver Email: <input  type="text" id="email" v-model="formData.level_appr_emails[0]" :disabled=!checkedCases.EEC placeholder="Approver1">  <br> 
          Default Approver Rules: <input type="checkbox" value=true :disabled=!checkedCases.EEC> rule1 <br>  

          Level2 Approver Email: <input  type="text" id="email" v-model="formData.level_appr_emails[1]" :disabled=!checkedCases.EEC placeholder="Approver2">  <br>
          Level2 Approver Rules: <input type="checkbox" value=true :disabled=!checkedCases.EEC> rule2 <br>  
        </div>
        <div class="form-actions">
          <button class="cancel" @click="cancelPage">Cancel</button>
          <button class="prev" @click="prevPage" >Previous</button>
          <button class="next" @click="nextPage" >Next</button>
        </div>
      </div>

      <div id="page_#" v-if="currentPageNumber == 4">
        <h3>Step3. Company credit cards expenses</h3>
        <div v-if="checkedCases.CCCE">
          <p>This is the content of Page 3.</p>
        </div>
        <div class="form-actions">
          <button class="cancel" @click="cancelPage">Cancel</button>
          <button class="prev" @click="prevPage" >Previous</button>
          <button class="next" @click="nextPage" >Next</button>
        </div>
      </div>
      
      <div id="page_4" v-if="currentPageNumber == 5">
        <h3>Step4. Bills Approval</h3>
        <div v-if="checkedCases.AB">
          <p>This is the content of Page 4.</p>
        </div>
        <div class="form-actions">
          <button class="prev" @click="prevPage" >Previous</button>
          <button class="next" @click="nextPage" >Next</button>
          <button class="cancel" @click="cancelPage">Cancel</button>
        </div>
      </div>
      <div id="page_5" v-if="currentPageNumber == 6">
        <h3>Step5. Review & Confirm</h3>
        <p>This is the content of Page 5.</p>
        <div class="form-actions">
          <button class="prev" @click="prevPage" >Previous</button>
          <button class="submit" @click="submitForm" >Submit</button>
          <button class="cancel" @click="cancelPage">Cancel</button>
        </div>
      </div>
      
    </div>
  </div>
</template>
  
<script>
import mitt from 'mitt'
const emitter = mitt()

export default {
  name: "PageControl",

  data() {
    return {
      currentPageNumber: 0,
      taskArr: [],
      checkedCases: {
        'EEC': false,
        'CCCE': false,
        'AB': true
      },
      formData: {
        dptmt_appr_names: [],
        dptmt_appr_emails: [],
        level_appr_emails: ["", ""]
      }
    };
  },
  computed: {
    isValidEmail(email) {
      return /^[^@]+@\w+(\.\w+)+\w$/.test(email);
    }
  },
  mounted () {
    emitter.on('next', this.nextPage)
    emitter.on('prev', this.prevPage)
    emitter.on('submit', this.submitForm)
    emitter.on('cancel', this.cancelPage)
  },
  methods: {
    startPage() {
      // console.log("Page data:", this.formData)
      this.currentPageNumber = 1;
    },
    nextPage(data) {
      console.log("Page data:", this.formData)
      this.currentPageNumber++;
      this.formData = Object.assign({}, this.formData, data)
    },
    prevPage(data) {
      this.currentPageNumber--;
      this.formData = Object.assign({}, this.formData, data)
    },
    cancelPage() {
      this.currentPageNumber = 0;
    },
    submitForm(data) {
      alert("Form submitted!");
      this.formData = Object.assign({}, this.formData, data)
      console.log("form data:", this.checkedCases, this.formData); // Log the final form data
      // you can post data to your backend here:
      // axios.post('/api/save-data', data)
      //   .then(response => {
      //     console.log(response.data)
      //   })
      //   .catch(error => {
      //     console.log(error.response.data)
      //   })
      this.currentPageNumber = 0;
    },
    addItem() {
        this.taskArr.push({
          name: "Adding",
        });
      },
    deleteItem(idx) {
      console.log(idx);
      this.taskArr.splice(idx, 1);
      this.formData.dptmt_appr_names.splice(idx, 1);
      this.formData.dptmt_appr_emails.splice(idx, 1);
    },
  },
};
</script>
