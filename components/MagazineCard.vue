<template>
  <div class="MagazineCard">
    <v-card outlined>
      <v-card-title>
        {{ items.series }}
      </v-card-title>
      <v-divider></v-divider>
      <v-card-subtitle>
        <v-btn outlined :to="{ name: 'series-id', params: { id: items.no } }">
          <v-icon small>fas fa-info-circle</v-icon>
          &nbsp;詳細</v-btn
        >
        <v-btn
          color="red"
          outlined
          :href="getPdfUrl(items.url)"
          target="_blank"
          rel="noopener"
        >
          <v-icon small>fas fa-file-pdf</v-icon>&nbsp;PDF</v-btn
        >
      </v-card-subtitle>
      <v-card-text>
        <v-row justify="space-between">
          <v-col cols="auto">
            <v-img v-bind:src="getThunmbnailUrl(items.image)" max-width="250" />
          </v-col>
          <v-col class="magazine-text">
            <time>{{ items.publishedDate }}</time
            >刊行 <br /><br />
            <div v-for="(article, i) in items.article" :key="i">
              <p>
                <a
                  :href="getPdfUrl(article.url)"
                  target="_blank"
                  rel="noopener"
                  >{{ getPaperText(article) }}</a
                >
              </p>
            </div>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      s3baseUrl:
        "https://nichelife-media.s3-ap-northeast-1.amazonaws.com/paper/",
    };
  },
  methods: {
    getPdfUrl(path) {
      var url = this.s3baseUrl + path;
      return url;
    },
    getThunmbnailUrl(path) {
      var url = require("@/assets/" + path);
      return url;
    },
    getPaperText(article) {
      var author = article.author ? " (" + article.author + ")" : "";
      return article.page + ": " + article.title + author;
    },
  },
};
</script>

<style lang="scss">
.MagazineCard {
  padding: 10px;
}

.magazine-text {
  padding: 20px;
}
</style>
