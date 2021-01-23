<template>
  <div class="container">
    <section class="filters">
      <button @click="toggle" style="margin: 1em">Toggle</button>
      <button @click="removeFilters" style="margin: 1em">Remove Filters</button>
      <div
        v-if="active"
        style="
          width: 100%;
          margin: auto;
          position: absolute;
          display: flex;
          justify-content: space-around;
          opacity: 0.85;
        "
      >
        <div class="author-filter">
          <p>Authors</p>
          <form @submit.prevent="handleSubmit">
            <div
              class="form-group form-check"
              v-for="item in Authors"
              v-bind:key="item.id"
            >
              <label class="form-check-label" :for="item.id">{{
                item.name
              }}</label>
              <input
                type="checkbox"
                v-model="user.authorCollection"
                :id="item.name"
                :value="item.name"
              />
            </div>

            <!-- print result -->
            <div class="form-group">
              {{ user.authorCollection }}
            </div>

            <div class="form-group">
              <button class="btn btn-primary">Submit</button>
            </div>
          </form>
        </div>

        <div class="title-filter">
          <p>Title</p>
          <form @submit.prevent="handleSubmit">
            <div
              class="form-group form-check"
              v-for="item in Titles"
              v-bind:key="item.id"
            >
              <label class="form-check-label" :for="item.id">{{
                item.name
              }}</label>
              <input
                type="checkbox"
                v-model="user.titleCollection"
                :id="item.name"
                :value="item.name"
              />
            </div>

            <!-- print result -->
            <div class="form-group">
              {{ user.titleCollection }}
            </div>

            <div class="form-group">
              <button class="btn btn-primary">Submit</button>
            </div>
          </form>
        </div>

        <div class="community-filter">
          <p>Communities</p>
          <form @submit.prevent="handleSubmit">
            <div
              class="form-group form-check"
              v-for="item in Communities"
              v-bind:key="item.id"
            >
              <label class="form-check-label" :for="item.id">{{
                item.name
              }}</label>
              <input
                type="checkbox"
                v-model="user.communityCollection"
                :id="item.name"
                :value="item.name"
              />
            </div>

            <!-- print result -->
            <div class="form-group">
              {{ user.communityCollection }}
            </div>

            <div class="form-group">
              <button class="btn btn-primary">Submit</button>
            </div>
          </form>
        </div>
      </div>
      <div class="author"></div>
      <div class="title"></div>
      <div class="content"></div>
      <div class="video"></div>
      <div class="communities"></div>
    </section>

    <section
      class="filtered-content"
      style="
        overflow: scroll;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        text-align: left;
        align-items: flex-start;
      "
    >
      <div
        class="article-wrapper"
        style="flex: 0 0 auto; margin: 10px"
        v-for="item in displayData"
        :key="item.ID"
      >
        <div class="author">
          <p class="author">{{ item.author }}</p>
        </div>
        <div class="title">
          <p class="author">{{ item.topic }}</p>
        </div>
        <div class="content">
          <p class="author">{{ item.content }}</p>
        </div>
        <div class="video">
          <p class="author">{{ item.author }}</p>
        </div>
        <div class="communities">
          <p class="author">{{ item.author }}</p>
        </div>
      </div>
    </section>
    <!-- <img class="cats" src="https://placekitten.com/g/200/300" alt="jacket" /> -->
  </div>
</template>

    <script>
import DataFrame from "dataframe-js";
export default {
  mounted() {
    this.fetchData();
  },
  methods: {
    handleSubmit() {
      alert(JSON.stringify(this.user));
    },
    toggle() {
      this.active = !this.active;
    },
    async fetchData() {
      this.df = await DataFrame.fromJSON("data1.json");
      // this.df.cast("Year of Deployment", Number);
      // this.df = this.df.fillMissingValues("NA", ["Manner of Procurement"]);
      this.df.show();
      this.displayData = this.df.toCollection();
      // this.constructModal();
    },
  },
  data() {
    return {
      displayData: [],
      active: true,
      Authors: [
        { name: "Apple" },
        { name: "Orange" },
        { name: "Mengo" },
        { name: "Cherry" },
      ],
      Titles: [
        { name: "Apple" },
        { name: "Orange" },
        { name: "Mengo" },
        { name: "Cherry" },
      ],
      Communities: [{ name: "Abd" }, { name: "Abdc" }, { name: "Abddd" }],
      user: {
        authorCollection: [],
        titleCollection: [],
        communityCollection: [],
        imgCollection: [],
        tagCollection: [],
        color: [],
      },
    };
  },

  name: "Mainpage",
  props: {
    msg: String,
  },
};
</script>

    <!-- Add "scoped" attribute to limit CSS to this component only -->
    <style>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.container {
  display: flex;
  flex-direction: column;
  grid-gap: 0 10%;
  justify-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
}

section {
  color: #eeeeee;
}

.filtered-content::after {
  display: block;
  content: " placeholder ";
  margin: 10px;
  flex: 999 999 auto;
}

.filtered-content > div:nth-child(2n + 1){
  width: 15%;
}
.filtered-content > div:nth-child(2n + 2){
  width: 20%;
}
.filtered-content > div:nth-child(2n + 3){
  width: 35%;
}
</style>
