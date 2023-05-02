<style>
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
  float: left;
}

.next {
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

.prev {
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
.submit {
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

.page-container {
  padding: 50px 100px;
  color: #3f51b5;
  background-color: #fdfdfd;
  border-radius: 10px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.3);
}

</style>

<script>
import PageControl from "./components/PageControl.vue";

export default {
  components: {
      PageControl,
  },
  data() {
    return {
      current: 'PageControl'
    }
  }
};
</script>


<template>
  <div>
    <h1>Setup your preferences:</h1>
    <!-- <PageControl /> -->
    <KeepAlive>
    <component :is="current"></component>
    </KeepAlive>  
  </div>
</template>