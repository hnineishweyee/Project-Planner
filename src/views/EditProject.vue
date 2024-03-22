<template>
    <h1>Edit Project</h1>
    <form @submit.prevent="addProject">
        <label>Project Title</label>
        <input type="text" v-model="title">
        <label>Project Detail</label>
        <input type="text" v-model="detail">
        <button @click="updateProject"> Update Project</button>
        <button @click="cancel"> Cancel </button>
    </form>
  </template>
  
  <script>
  export default {
    props:["id"],

    data(){
        return{
           title:"",
           detail:""
        }
    },
    mounted(){
      fetch('http://localhost:3000/projects/'+this.id)
      .then((res)=>{
          return res.json()
          // console.log(res);
      })
      .then((datas)=>{
        this.title=datas.title
        this.detail=datas.detail
      })
      .catch((err)=>{
        console.log(err);
      })
    },
    methods:{
      updateProject(){
          fetch('http://localhost:3000/projects/'+this.id,{
            method:"PATCH",
            headers:{
              "Content-type":"application/json"
            },
            body:JSON.stringify(    // Change ချင်တဲ့ Data တွေထည့်ပေးရ
              {
                title:this.title,
                detail:this.detail
              }
            )
          })
          // REDIRECT
          .then(()=>{
              this.$router.push("/")
          })
          .catch((err)=>{
            console.log(err);
          })
        
      },
      cancel(){
        // REDIRECT
        this.$router.push("/")
      }
    } 
  }
  </script>
  
  <style scoped>
  form {
      background: white;
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
      margin: 20px 0 10px 0
    }
    input {
      padding: 10px;
      border: 0;
      border-bottom: 1px solid #ddd;
      width: 100%;
      box-sizing: border-box;
    }
    textarea {
      border: 1px solid #ddd;
      padding: 10px;
      width: 100%;
      box-sizing: border-box;
      height: 100px;
    }
    form button {
      display: block;
      margin: 20px auto 0;
      background: #00aeff;
      color: white;
      padding: 10px;
      border: 0;
      border-radius: 6px;
      font-size: 16px;
    }
    button:hover{
    background-color: #009fe8;
    }
  </style>