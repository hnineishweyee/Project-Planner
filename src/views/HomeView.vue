<template>
  <div class="home">
    <h1> Home </h1>

    <FilterNav @filterValue="current=$event" :current="current"> </FilterNav>
    
    <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="deleteProject" @complete="completeProject"></SingleProject>
    </div>
  </div>
</template>

<script>

import SingleProject from '@/components/SingleProject.vue'
import FilterNav from '@/components/FilterNav.vue'

export default {
  name: 'HomeView',

  components: { SingleProject, FilterNav },
  
  data(){
    return {
      projects:[],
      current: "all"
    }
  },

  methods:{
    deleteProject(id){
      this.projects=this.projects.filter(project=>{
        return project.id!=id;
      })
    },
    completeProject(id){
      let findProject=this.projects.find(project=>{
        return project.id===id;
      })
      findProject.complete=!findProject.complete
    }
  },

  computed:{
      filteredProjects(){
         if(this.current==="complete"){
           return this.projects.filter((proj)=>{
             return proj.complete
           })
         }
         if(this.current==="ongoing"){
          return this.projects.filter((proj)=>{
             return !proj.complete  // completed မဖြစ်တာ
           })
         }
         return this.projects;
      }
  },

  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response)=>{
      return response.json()
    })

    .then((datas)=>{
      this.projects = datas
}
)
    .catch(()=>{

    })
  }
}
</script>