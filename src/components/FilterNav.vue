<template>
    <nav class="filter-nav">
        <button @click="filterBy('all')" :class="{ active : current==='all'}"> All Projects </button>
        <button @click="filterBy('complete')" :class="{ active : current==='complete'}"> Completed </button>
        <button @click="filterBy('ongoing')" :class="{ active : current==='ongoing'}"> Ongoing </button>
    </nav>
  </template>
  
  <script>
  export default {
      props:['current'],
      
      methods:{
          filterBy(value){
             this.$emit("filterValue",value);
          }
      }
  }
  </script>
  
  <style>
      .filter-nav button {
          background: none;
          border: none;
          color: #bbb;
          outline: none;
          font-size: 12px;
          text-transform: uppercase;
          margin-right: 10px;
          letter-spacing: 1px;
          font-weight: bold;
          cursor: pointer;
      }
      button.active {
          color: #333;
      }
  </style>