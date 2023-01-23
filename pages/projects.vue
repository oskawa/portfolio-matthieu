<template>
  <section class="projects">
    <div class="container-fluid projects-container">
      <div class="row projects-row">
        <div class="col-md-11 col-12 offset-md-1">
          <h1>Projet(s)</h1>
          <div class="projects-informations">
            <div class="projects-informations__single">
              <p class="projects-informations__title">Type</p>
              <ul>
                <li>Website</li>
                <li>Plateforme Saas</li>
                <li>E-commerce</li>
                <li>Web App</li>
              </ul>
            </div>
            <div class="projects-informations__single">
              <p class="projects-informations__title">Années</p>
              <ul>
                <li>2019-2023</li>
              </ul>
            </div>
            <div class="projects-informations__single">
              <p class="projects-informations__title">Discipline</p>
              <ul>
                <li>UI Design</li>
                <li>UX Research</li>
                <li>Direction Artistique</li>
                <li>Développement Web</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
      <div class="row projects-list">
        <div class="col-md-10 col-12 offset-md-1">
          <div class="projects-single" v-for="(project, index) in projects" :key="project.id">

            <nuxt-link class="projects-single__link" :to="`/project/${project.id}`" >
            <div class="projects-single__title">
                <h2 class="projects-single__title_number">0{{index+1}}</h2>
                <h2>{{project.acf.project_title}}</h2>
              </div>
              <div class="projects-single__type">
                <h5>{{project.acf.project_type}}<span v-if=project.acf.application_type>, {{project.acf.application_type}}</span></h5>
              </div>
            </nuxt-link>
          </div>
         
          
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import axios from "axios";

export default {
  head() {
    return {
      title: "Portfolio Matthieu Delomez",
      meta: [
        {
          hid: "description",
          name: "description",
          content:
            "Une sélection de projets !",
          hid: "description",
        },
      ],
    };
  },
  data() {
    return {
      about_text: "",
      projects : [],
    };
  },
  mounted() {
    let endpoints = [
      process.env.wordpressAPI + "wp/v2/portfolio",
      process.env.wordpressAPI + "acf/v3/options/options",
    ];

    axios.all(endpoints.map((endpoint) => axios.get(endpoint))).then((response) => {
      console.log(response);
      this.projects = response[0].data
    
      
    });
  },
};
</script>
