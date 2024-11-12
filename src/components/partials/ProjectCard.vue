<script>

import axios from 'axios';

export default{
  data(){
    return{
      projects:[],
      prevPage: null,
      nextPage: null,
      clickedButton: false,
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
          this.prevPage = res.data.projects.prev_page_url;
          this.nextPage = res.data.projects.next_page_url;
        });
    },
    toPrevPage(){
      this.clickedButton = true;
      axios
        .get(this.prevPage)
        .then((res)=>{
          this.projects = res.data.projects.data;
          console.log(this.projects);
          this.prevPage = res.data.projects.prev_page_url;
          this.nextPage = res.data.projects.next_page_url;

          this.clickedButton = false;
        });
    },
     toNextPage(){
      this.clickedButton = true;
      axios
        .get(this.nextPage)
        .then((res)=>{
          this.projects = res.data.projects.data;
          console.log(this.projects);
          this.prevPage = res.data.projects.prev_page_url;
          this.nextPage = res.data.projects.next_page_url;

          this.clickedButton = false;
        });
    },
  },
}

</script>

<template>

  <main>

    <div class="container py-5">

      <div class="row">

        <div class="col" v-for="project in projects" :key="project.id">

          <div class="card mb-4">

            <img :src="project.cover" class="card-img-top" :alt="project.title">

            <div class="card-body">
                <h5 class="card-title">
                  {{project.id}} {{project.title}}
                </h5>

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

        <div class="d-flex justify-content-center">

          <button 
            @click="toPrevPage()"
            :disabled="prevPage == null || clickedButton" 
            type="button" 
            class="btn btn-primary m-2">
            Prev
          </button>

          <button
            @click="toNextPage()"
            :disabled="nextPage == null || clickedButton" 
            type="button"
            class="btn btn-primary m-2">
            Next
          </button>

        </div>

      </div>

    </div>

  </main>

</template>