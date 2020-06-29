<template>
  <div class="page">
    <h1 class="text-center">Home</h1>
    <div class="container">
      <BaseLayout>
        <template v-slot:left>
          <b-nav vertical pills>
            <b-nav-item
              v-for="s in sections"
              :key="s"
              :active="s == selectedSection"
              @click="selectedSection = s; getAllArticles()"
            >{{s}}</b-nav-item>
          </b-nav>
        </template>
        <template v-slot:section-dropdown>
          <b-form-select
            v-model="selectedSection"
            :options="sections"
            @change="getAllArticles()"
            id="section-dropdown"
          ></b-form-select>
        </template>
        <template v-slot:right>
          <div class="row">
            <div class="col-md-4 mb-3" v-for="(a, index) in articles" :key="index">
              <b-card
                :title="a.title"
                :img-src="(Array.isArray(a.multimedia) && a.multimedia.length > 0 && a.multimedia[a.multimedia.length-1].url) || ''"
                img-top
              >
                <b-card-text>
                  <p>{{a.byline}}</p>
                  <p>Published on: {{new Date(a.published_date) | dateFormat('YYYY.MM.DD hh:mm a')}}</p>
                  <p>{{a.abstract}}</p>
                </b-card-text>
                <b-button :href="a.short_url" variant="primary" target="_blank">Go</b-button>
              </b-card>
            </div>
          </div>
        </template>
      </BaseLayout>
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
import BaseLayout from "@/components/BaseLayout.vue";
import { requestsMixin } from "@/mixins/requestsMixin";
export default {
  name: "home",
  components: {
    BaseLayout
  },
  mixins: [requestsMixin],
  data() {
    return {
      sections: `arts, automobiles, books, business, fashion,
      food, health, home, insider, magazine, movies, national,
      nyregion, obituaries, opinion, politics, realestate, science,
      sports, sundayreview, technology, theater,
      tmagazine, travel, upshot, world`
        .split(",")
        .map(s => s.trim()),
      selectedSection: "arts",
      articles: []
    };
  },
  beforeMount() {
    this.getAllArticles();
  },
  methods: {
    async getAllArticles() {
      const response = await this.getArticles(this.selectedSection);
      this.articles = response.data.results;
    },
    setSection(ev) {
      this.getAllArticles();
      console.log(ev);
    }
  }
};
</script>
<style scoped>
#section-dropdown {
  margin-bottom: 10px;
}
</style>