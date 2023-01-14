<template>
  <section class="home">
    <div class="home-container container-fluid">
      <div class="home-row row">
        <div
          class="home-col col-12 col-xxl-6 col-xl-5 offset-xl-1 offset-lg-1 col-lg-6 col-md-8 home-col"
        >
          <h1 v-html="this.title">
          
          </h1>
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
      title: "Portfolio Maxime Eloir - Homepage",
      meta: [
        {
          hid: "description",
          name: "description",
          content:
            "My portfolio ! A selection of print & web projects and missions carried out during my studies and my business.",
          hid: "description",
        },
      ],
    };
  },
  data(){
    return{
      title:"",
           
  }
  },
  mounted() {
   
    axios
      .get(process.env.wordpressAPI + "wp/v2/pages?slug=accueil")
      .then((response) => {
        console.log(response.data)
        this.title = response.data[0].acf.page_title        
      });
  },
};
</script>

<style>
.home {
  background: #1a1a1a;
}
</style>
