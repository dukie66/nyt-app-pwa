<template>
  <div class="page">
    <h1 class="text-center">Search</h1>
    <div class="container">
      <SearchLayout>
        <template v-slot:top>
          <!-- <ValidationObserver ref="observer" v-slot="{ invalid }">
          <b-form @submit.prevent="onSubmit" novalidate id="form">
            <b-form-group label="Keyword" label-for="keyword">
              <ValidationProvider name="keyword" rules="required" v-slot="{ errors }">
                <b-form-input
                  :state="errors.length == 0"
                  v-model="form.keyword"
                  type="text"
                  required
                  placeholder="Keyword"
                  name="keyword"
                ></b-form-input>
                <b-form-invalid-feedback :state="errors.length == 0">Keyword is required</b-form-invalid-feedback>
              </ValidationProvider>
            </b-form-group>
            <b-button type="submit" variant="primary">Search</b-button>
          </b-form>
          </ValidationObserver>-->
          <ValidationObserver ref="observer">
            <b-form @submit.prevent="onSubmit" novalidate id="form">
              <b-form-group label="Keyword" label-for="keyword">
                <ValidationProvider name="keyword" rules="required" v-slot="{ errors }">
                  <b-form-input
                    :state="errors.length == 0"
                    v-model="form.keyword"
                    type="text"
                    required
                    placeholder="Keyword"
                    name="keyword"
                  ></b-form-input>
                  <b-form-invalid-feedback :state="errors.length == 0">Keyword is required</b-form-invalid-feedback>
                </ValidationProvider>
              </b-form-group>
              <b-button type="submit" variant="primary">Search</b-button>
            </b-form>
          </ValidationObserver>
        </template>
        <template v-slot:bottom>
          <b-card v-for="(a, index) in articles" :key="index" :title="a.headline.main">
            <b-card-text>
              <p>By: {{a.byline.original}}</p>
              <p>Published on: {{new Date(a.pub_date) | dateFormat('YYYY.MM.DD hh:mm a')}}</p>
              <p>{{a.abstract}}</p>
            </b-card-text>
            <b-button :href="a.web_url" variant="primary" target="_blank">Go</b-button>
          </b-card>
        </template>
      </SearchLayout>
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
import SearchLayout from "@/components/SearchLayout.vue";
import { requestsMixin } from "@/mixins/requestsMixin";
export default {
  name: "home",
  components: {
    SearchLayout
  },
  mixins: [requestsMixin],
  data() {
    return {
      articles: [],
      form: {}
    };
  },
  methods: {
    async onSubmit() {
      const isValid = await this.$refs.observer.validate();
      if (!isValid) {
        return;
      }
      const response = await this.searchArticles(this.form.keyword);
      this.articles = response.data.response.docs;
    }
  }
};
</script>
<style scoped>
</style>