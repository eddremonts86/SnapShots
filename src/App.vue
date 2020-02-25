<template>
  <div id="container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="#">Snap-Shots</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">
              Home
              <span class="sr-only">(current)</span>
            </a>
          </li>
        </ul>
        <form class="form-inline my-2 my-lg-0">
          <div class="form-check mx-2">
            <input
              class="form-check-input"
              type="radio"
              name="exampleRadios"
              id="exampleRadios1"
              v-model="tags"
              :value="tags"
            />
            <label class="form-check-label" for="exampleRadios1">Tags</label>
          </div>
          <div class="form-check mx-2">
            <input
              class="form-check-input"
              type="radio"
              name="exampleRadios"
              id="exampleRadios2"
              v-model="ids"
              :value="ids"
              checked
            />
            <label class="form-check-label" for="exampleRadios2">Ids</label>
          </div>

          <input
            class="form-control mr-sm-2"
            type="search"
            placeholder="Search"
            aria-label="Search"
            v-model="elementToPrint"
          />
          <snapshopt :eleID="eleID" :eleTag="eleTag" :iframeTag="iframeTag" />
        </form>
      </div>
    </nav>
    <section :key="key" class="imageSettion" v-for="(imgs, key) in images">
      <div class="flex">
        <div
          class="card"
          style="width: 18rem;"
          :key="img.id"
          v-for="img in imgs"
        >
          <img :src="img.download_url" width="100%" class="images" />
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import snapshopt from "@/components/snapShots.vue";
import axios from "axios";

export default {
  name: "App",
  components: { snapshopt },
  data: () => {
    return {
      filter: true,
      elementToPrint: "",
      iframeTag: false,
      eleID: false,
      eleTag: false,
      ids: true,
      tags: false,
      images: []
    };
  },
  created() {
    this.getImages();
  },
  methods: {
    getElemet() {
      switch (this.ids) {
        case true:
          this.eleID = this.elementToPrint;
          this.eleTag = false;

          break;
        case false:
          this.eleID = false;
          this.eleTag = this.elementToPrint;

          break;
        default:
        // code block
      }
    },
    getImages() {
      axios({
        method: "get",
        url: "https://picsum.photos/v2/list"
      }).then(response => {
        this.images = this.chunk(response.data, 5);
      });
    },
    chunk(arr, len) {
      var chunks = [],
        i = 0,
        n = arr.length;
      while (i < n) {
        chunks.push(arr.slice(i, (i += len)));
      }
      return chunks;
    }
  },
  watch: {
    elementToPrint() {
      this.getElemet();
    }
  }
};
</script>

<style lang="scss" scope>
@import url("https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.imageSettion {
  padding: 2%;
  margin: 2%;
  background: rgba(194, 194, 194, 0.363);
  border-radius: 14px;
}
.flex {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
.images {
  border-radius: 5px;
}
</style>
