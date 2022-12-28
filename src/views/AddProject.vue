<template>
  <form @submit.prevent="handleSubmit">
    <label>Title: </label>
    <input type="text" v-model="title" required/>
    <label>Details: </label>
    <textarea v-model="details" required></textarea>
    <button>Add Project</button>
  </form>
</template>
<script>
import axios from 'axios'
export default{
  data(){
    return{
      title:'',
      details:''
    }
  },
  methods:{
    async handleSubmit(){
      try{
        let project = {
          title: this.title,
          details: this.details,
          complete: false
        }
        const { data } = await axios.post("http://localhost:3000/projects", project);
        await this.$router.push('/')
      }catch(err){
        console.log(err)
      }
    }
  }
}
</script>
<style>
form {
  background: #2f4765;
  padding: 20px;
  border-radius: 10px;
}
label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  font-family: 'Open Sans', sans-serif;
  font-size: 16px;
  padding: 10px;
  border: 0;
  border-bottom: 2px solid #ddd;
  width: 100%;
  box-sizing: border-box;
  background-color: #2f4765;
  color: #bbb;
}
textarea {
  font-family: 'Open Sans', sans-serif;
  font-size: 16px;
  background-color: #2f4765;
  border: 2px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
  color: #bbb;
}
form button {
  display: block;
  margin: 20px auto 0;
  background: #35df90;
  color: #fff;
  padding: 10px;
  border: 0;
  border-radius: 7px;
  font-size: 16px;
}
</style>