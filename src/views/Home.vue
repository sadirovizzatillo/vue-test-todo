<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"></FilterNav>
    <div v-if="projects.length">
      <div v-for="project in filterProjects" :key="project.id">
        <SingleProject 
        :project="project"
        @delete="handleDelete"
        @complete="handleComplete"></SingleProject>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '@/components/FilterNav.vue'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  data(){
    return{
      projects:[],
      current:'all'
    }
  },
  async mounted(){
    try{
      const { data } = await axios.get('http://localhost:3000/projects');
      this.projects = data
    }catch(err){
      console.log(err)
    }
  },
  methods:{
    handleDelete(id){
      this.projects = this.projects.filter((project)=>project.id !==id)
    },
    handleComplete(id){
      let p = this.projects.find((project)=>project.id===id)
      p.complete = !p.complete 
    }
  },
  computed:{
    filterProjects(){
      if(this.current === 'completed'){
        return this.projects.filter((project)=> project.complete)
      }
      if(this.current === 'ongoing'){
        return this.projects.filter((project)=> !project.complete)
      }
      return this.projects
    }
  }
}
</script>
