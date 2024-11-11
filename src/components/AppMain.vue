<script>

import axios from 'axios';

export default{
  data(){
    return{
      projects:[],
      prevPage: null,
      nextPage: null,
    };
  },
  mounted(){
    this.getProjects();
  },
  methods:{
    getProjects(){
      axios
        .get('http://127.0.0.1:8000/api/projects')
        .then((res)=>{
          this.projects = res.data.projects.data;
          console.log(this.projects);
          

          
        });
    }
  },
}

</script>

<template>

  <main>

    <div class="container py-5">

      <div class="row">

        <div class="col" v-for="project in projects" :key="project.id">

          <div class="card mb-4" style="width: 18rem;">

            <img :src="project.cover" class="card-img-top" :alt="project.title">

            <div class="card-body">
                <h5 class="card-title">{{project.title}}</h5>
                <p class="card-text">{{project.description}}</p>

                <div class="mb-3" v-if="project.type !=null">
                  {{ project.type.title }}
                </div>
                <div class="mb-3" v-else>
                  Without Type
                </div>

                <span v-for="technology in project.technologies" :key="technology.id" class="badge rounded-pill text-bg-primary me-2">
                  {{ technology.title }}
                </span>
            </div>

          </div>

        </div>

      </div>

    </div>

  </main>

</template>

<style scoped>
</style>