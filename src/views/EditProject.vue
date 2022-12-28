<template>
  <form @submit.prevent="handleUpdate">
    <label>Title: </label>
    <input type="text" v-model="title" required/>
    <label>Details: </label>
    <textarea v-model="details" required></textarea>
    <button>Update Project</button>
  </form>
</template>
<script>
import axios from 'axios'
export default{
  props:['id'],
  data(){
    return{
      title:'',
      details:'',
      uri: 'http://localhost:3000/projects/' + this.id
    }
  },
  async mounted(){
    try{
      const { data } = await axios.get(this.uri);
      this.title= data.title,
      this.details= data.details
    }catch(err){
      console.log(err)
    }
  },    
  methods:{
    async  handleUpdate(){
      try{
        const { data } = await axios.patch(this.uri, {
          title: this.title, 
          details: this.details});
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
  padding: 10px;
  font-family: 'Open Sans', sans-serif;
  font-size: 16px;
  background-color: #2f4765;
  color: #bbb;
  border: 0;
  border-bottom: 2px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}
textarea {
  font-family: 'Open Sans', sans-serif;
  font-size: 16px;
  border-style: none;
  background-color: #2f4765;
  border-bottom: 2px solid #ddd;
  color: #bbb;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
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