<template>
  <section class="contact">
    <div class="contact-container container-fluid">
      <div class="contact-row row">
        <div
          class="offset-xxl-1 col-xxl-6 offset-xl-1 col-xl-6 offset-lg-1 col-lg-5 col-md-6 col-12"
        >
          <h3>{{this.option_name}}</h3>
          <div class="informations">
            <p class="profession">{{this.option_profession}}</p>
            <a class="mail" :href="`mailto:${this.option_mail}`">{{this.option_mail}}</a
            >
            <p class="localisation">{{this.option_city}}</p>
          </div>
        </div>
        <div
          class="col-xxl-4 col-xl-5 col-lg-5 col-md-6 col-12"
          v-html="this.about_text"
        ></div>
      </div>
    </div>

    <div class="contact-bottom container-fluid">
      <div class="contact-bottom__row row">
        <div class="col-10 contact-bottom__col">
          <div class="contact-date">
            <p>{{ new Date().getFullYear() }}</p>
          </div>
          <div class="contact-socials">
            <ul>
              <li v-if="this.option_dribble">
                <a :href="this.option_dribble">Dr</a>
              </li>
              <li v-if="this.option_behance">
                <a :href="this.option_behance">Be</a>
              </li>
              <li v-if="this.option_linkedin">
                <a :href="this.option_linkedin">Li</a>
              </li>
             
            </ul>
          </div>
          <div class="contact-cv">
            <a href="">Curriculum Vitae</a>
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
            "My portfolio ! A selection of print & web projects and missions carried out during my studies and my business.",
          hid: "description",
        },
      ],
    };
  },
  data() {
    return {
      about_text: "",
      option_name: "",
      option_profession: "",
      option_mail: "",
      option_city: "",
      option_dribble: "",
      option_behance: "",
      option_linkedin: "",
      
    };
  },
  mounted() {
    
    let endpoints = [
      process.env.wordpressAPI + "wp/v2/pages?slug=a-propos",
     process.env.wordpressAPI + "acf/v3/options/options"
    ];

    axios
      .all(endpoints.map((endpoint) => axios.get(endpoint)))
      .then((response) => {
        console.log(response)
       this.about_text = response[0].data[0].acf.about_text;
       this.option_name = response[1].data.acf.titre_de_la_personne
       this.option_profession =  response[1].data.acf.poste_occupe
       this.option_mail = response[1].data.acf.adresse_mail
       this.option_city = response[1].data.acf.ville_actuelle
       this.option_dribble = response[1].data.acf.dribble
       this.option_behance = response[1].data.acf.behance
       this.option_linkedin = response[1].data.acf.linkedin
      });
  },
};
</script>
