<template>
  <section class="single-project">
    <div class="single-project__title">
      <h1>{{ this.project.project_title }}</h1>
      <div class="single-project__title-sub">
        <h6>{{ this.project.project_type }}</h6>
        <hr />
        <h6>{{ this.project.application_type }}</h6>
      </div>
    </div>

    <div v-for="content in project.repeatable_content">
      <div
        v-if="content.acf_fc_layout === 'classic_image'"
        class="acf-img-full container"
      >
        <div class="row acf-img-full__row">
          <div class="col-12 col-lg-10">
            <img :src="content.image.url" alt="" />
          </div>
        </div>
      </div>

      <div
        v-if="content.acf_fc_layout === 'title_text_content'"
        class="acf-text-title container"
      >
        <div class="row">
          <div class="offset-md-1 col-md-5">
            <h3>{{ content.title }}</h3>
          </div>
          <div class="col-md-5" v-html="content.content"></div>
        </div>
      </div>

      <div v-if="content.acf_fc_layout === 'images_typo'" class="acf-img-typo container">
        <div class="row acf-img-typo__row">
          <div
            class="col-md-5 col-12"
            v-for="typo in content.typo_image_single"
            :key="typo.ID"
          >
            <img :src="typo.sizes.medium_large" alt="" />
          </div>
        </div>
      </div>
      <div
        v-if="content.acf_fc_layout === 'application_color'"
        class="acf-img-colors container"
      >
        <div class="row">
          <div
            v-for="(color, index) in content.colors"
            :key="index"
            :class="index > 1 ? 'col-md-4 col-12' : 'col-md-6 col-12'"
          >
            <div
              class="img-colors__content"
              :style="`background-color:${color.hexa_color}`"
            >
              <span :class="color.is_black ? 'isBlack' : 'isWhite'">{{
                color.titre
              }}</span
              ><br />
              <span :class="color.is_black ? 'isBlack' : 'isWhite'">{{
                color.hexa_color
              }}</span>
            </div>
          </div>
        </div>
      </div>
      <div v-if="content.acf_fc_layout === 'outside_image_grid'" class="acf-outside-grid container-fluid">
        <div class="row acf-outside-grid__row">
          <div
            class="col-12"
            v-for="outside_image in content.wireframe_userflow_gallery"
            :key="outside_image.ID"
          >
            <img :src="outside_image.url" alt="" />
          </div>
        </div>
      </div>
    </div>

    <!-- <div class="acf-img-colors container-fluid">
      <div class="row">
        <div class="col-md-6 col-12">
          <div class="img-colors__content">
            <span>Couleur principale 01</span><br />
            <span>#361019</span>
          </div>
        </div>
        <div class="col-md-6 col-12">
          <div class="img-colors__content">
            <span>Couleur principale 01</span><br />
            <span>#361019</span>
          </div>
        </div>
        <div class="col-md-4 col-12">
          <div class="img-colors__content">
            <span>Couleur principale 01</span><br />
            <span>#361019</span>
          </div>
        </div>
        <div class="col-md-4 col-12">
          <div class="img-colors__content">
            <span>Couleur principale 01</span><br />
            <span>#361019</span>
          </div>
        </div>
        <div class="col-md-4 col-12">
          <div class="img-colors__content">
            <span>Couleur principale 01</span><br />
            <span>#361019</span>
          </div>
        </div>
      </div>
    </div> -->
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
      id: this.$route.params.id,
      project: [],
    };
  },
  mounted() {
    let endpoints = [
      process.env.wordpressAPI + "wp/v2/portfolio/" + this.id,
      process.env.wordpressAPI + "acf/v3/options/options",
    ];

    axios.all(endpoints.map((endpoint) => axios.get(endpoint))).then((response) => {
      this.project = response[0].data.acf;
      console.log(this.project);
    });
  },
};
</script>
