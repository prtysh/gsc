<template>
  <div class="container">
    <section class="filters">
      <button @click="toggle" style="margin: 1em">Toggle Filters</button>
      <button @click="removeFilters" style="margin: 1em">
        Remove All Filters
      </button>
      <div
        v-if="active"
        style="
          width: 100%;
          margin: auto;

          display: flex;
          justify-content: space-around;
          opacity: 0.85;
        "
      >
        <div class="author-filter">
          <h3>Authors</h3>
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
                @change="convertFilters"
              />
            </div>

            <!-- print result -->
            <div class="form-group">
              <!-- {{ user.authorCollection }} -->
            </div>

            <div class="form-group">
              <!-- <button class="btn btn-primary">Submit</button> -->
            </div>
          </form>
        </div>

        <div class="title-filter">
          <h3>Title</h3>
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
                @change="convertFilters"
              />
            </div>

            <!-- print result -->
            <div class="form-group">
              <!-- {{ user.titleCollection }} -->
            </div>

            <div class="form-group">
              <!-- <button class="btn btn-primary">Submit</button> -->
            </div>
          </form>
        </div>

        <div class="community-filter">
          <h3>Communities</h3>
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
                @change="convertFilters"
              />
            </div>

            <!-- print result -->
            <div class="form-group">
              <!-- {{ user.communityCollection }} -->
            </div>

            <div class="form-group">
              <!-- <button class="btn btn-primary">Submit</button> -->
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
        style="padding: 3em"
        v-for="item in displayData"
        :key="item.ID"
      >
        <div v-if="item.type === 'paragraph'">
          <div class="author">
            <h5 class="author">{{ item.author }}</h5>
          </div>
          <div class="title">
            <h6 class="topic">{{ item.topic }}</h6>
          </div>
          <div class="content">
            <p class="content" v-bind:class="item.color">
              {{ item.content }}
            </p>
            <blockquote v-if="item.type === 'pullquote'">
              {{ item.content }}
            </blockquote>
            <!-- <p class="content" v-bind:class="item.">{{ item.content }}</p> -->
          </div>
          <div class="video">
            <!-- <p class="video">{{ item.video }}</p> -->
          </div>
          <div class="communities">
            <!-- <p class="community">{{ item.community }}</p> -->
          </div>
        </div>

        <div v-if="item.type === 'pullquote'">
          <div class="content">
            <blockquote>"{{ item.content }}"</blockquote>

            <div class="author">
              <h5 style="text-align: right; padding-right: 3rem" class="author">
                - {{ item.author }}
              </h5>
            </div>
          </div>
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
    removeFilters() {
      this.displayData = this.df.toCollection();
    },
    convertFilters() {
      let filters = JSON.parse(JSON.stringify(this.user));

      // filter by author

      let d = [];
      for (let i = 0; i < filters.authorCollection.length; i++) {
        let v = filters.authorCollection[i];
        let k = "author";
        let o = { filterCriteria: k, filterOption: v };
        d.push(o);
      }

      // filter by topics

      for (let i = 0; i < filters.titleCollection.length; i++) {
        let v = filters.titleCollection[i];
        let k = "topic";
        let o = { filterCriteria: k, filterOption: v };
        d.push(o);
      }

      // filter by communities

      for (let i = 0; i < filters.communityCollection.length; i++) {
        let v = filters.communityCollection[i];
        let k = "communities";
        let o = { filterCriteria: k, filterOption: v };
        d.push(o);
      }

      console.log(d);
      this.constructFilterDF(d);
      this.dataFiltering();
    },
    constructFilterDF(data) {
      this.filterDf = new DataFrame(data, ["filterCriteria", "filterOption"]);
      // this.filterDf.show();
      this.filterArray = this.filterDf.toArray();
      console.log(this.filterArray);
    },
    dataFiltering() {
      this.displayData = [];
      for (let i = 0; i < this.filterArray.length; i++) {
        // console.log(typeof this.filterArray[i][0]);
        // console.log(typeof this.filterArray[i][1]);
        let colName = this.filterArray[i][0];
        let rowValue = this.filterArray[i][1];
        if (this.displayData.length === 0) {
          this.displayData = this.df.filter((row) =>
            row.get(colName).toString().includes(rowValue)
          );
        } else {
          let dftemp = this.df.filter((row) =>
            row.get(colName).toString().includes(rowValue)
          );
          this.displayData = this.displayData.union(dftemp);
          this.displayData = this.displayData.dropDuplicates();
        }
      }
      // this.displayData.select("Name", "Year of Deployment", "ID").show();
      this.displayData = this.displayData.toCollection();
    },
    handleSubmit() {
      alert(JSON.stringify(this.user));
    },
    toggle() {
      this.active = !this.active;
    },
    getAuthors() {
      let d = [];
      let arr = this.df.unique("author").toArray();
      for (let i = 0; i < arr.length; i++) {
        let o = { name: arr[i][0] };
        d.push(o);
      }
      this.Authors = d;
    },
    getTopics() {
      let d = [];
      let arr = this.df.unique("topic").toArray();
      for (let i = 0; i < arr.length; i++) {
        let o = { name: arr[i][0] };
        d.push(o);
      }
      this.Titles = d;
    },
    getCommunities() {
      let d = [];
      let arr = this.df.unique("communities").toArray();
      for (let i = 0; i < arr.length; i++) {
        let o = { name: arr[i][0] };
        d.push(o);
      }
      this.Communities = d;
    },
    async fetchData() {
      this.df = await DataFrame.fromJSON("data2.json");
      this.df.show();
      this.getAuthors();
      this.getTopics();
      this.getCommunities();
      this.displayData = this.df.toCollection();
    },
  },

  data() {
    return {
      yellow: true,
      displayData: [],
      active: true,
      Authors: [
        { name: "SP" },
        { name: "LP" },
        { name: "RJO" },
        { name: "GH" },
      ],
      Titles: [
        { name: "Challenges faced by stakeholders" },
        { name: "Mobility challenges created by the lockdown" },
        { name: "Relief efforts" },
        { name: "Gaps in government data" },
      ],
      Communities: [
        { name: "urban poor & homeless" },
        { name: "schoolchildren" },
        { name: "general" },
        { name: "women" },
      ],

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
  margin: 10px;
  flex: 999 999 auto;
}

.filtered-content > div:nth-child(5n + 1) {
  width: 15%;
  padding: 1em;
}
.filtered-content > div:nth-child(5n + 2) {
  width: 20%;
}
.filtered-content > div:nth-child(5n + 3) {
  width: 35%;
}
.filtered-content > div:nth-child(5n + 4) {
  width: 25%;
}
.filtered-content > div:nth-child(5n + 5) {
  width: 45%;
}
p {
  font-size: 12px;
}

/* Hide scrollbar for Chrome, Safari and Opera */
.filtered-content::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.filtered-content {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}

@media only screen and (max-width: 1081px) {
  .filtered-content > div:nth-child(2n + 1) {
    width: 60%;
  }
  .filtered-content > div:nth-child(2n + 2) {
    width: 70%;
  }
  .filtered-content > div:nth-child(2n + 3) {
    width: 55%;
  }
}

button {
  background-color: #2c3e50;
  color: #eeeeee;
  border: none;
  padding: 5px 20px;
}

p {
  border-left: #eeeeee 1px solid;
  padding-left: 1em;
}

.pink {
  border-left: #ea62d8 2px double;
}

.yellow {
  border-left: #eacb4f 2px double;
}

.orange {
  border-left: #ea7e4f 1px double;
}

.purple {
  border-left: #9b42f4 1px double;
}

.red {
  border-left: #ef1a4c 1px double;
}

.brown {
  border-left: #512818 1px double;
}

.green {
  border-left: #7dba47 1px double;
}

.cyan {
  border-left: #23d7eb 1px double;
}
</style>
