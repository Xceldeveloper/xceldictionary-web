<template>
  <div id="wrapperx">
    <v-toolbar
      width="100%"
      height="60px"
      dark
      color="#000"
      elevation="1"
      dense
      floating
    >
      <v-btn icon @click="$router.go(-1)">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-text-field
        hide-details
        elevation="0"
        v-model="searchQuery"
        prepend-inner-icon="mdi-magnify"
        single-line
        solo
      ></v-text-field>

      <v-btn icon>
        <v-icon>mdi-microphone</v-icon>
      </v-btn>
    </v-toolbar>
    <v-progress-linear
      v-if="loading && searchQuery.length > 0"
      height="2px"
      color="#000"
      indeterminate
      width="100%"
    />
    <div class="search-result-cover">
      <v-list v-if="searchQuery.length == 0 && histories.length > 0">
        <v-list-item
          link
          v-for="(history, index) in histories"
          :key="index"
          @click="setText(history)"
        >
          <v-list-item-icon>
            <v-icon>mdi-history</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            {{ history }}
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-list v-if="searchQuery.length > 0 && autoCompleted.length > 0">
        <v-list-item
          link
          v-for="(autoComplete, index) in autoCompleted"
          :key="index"
          @click="addToSearchHistory(autoComplete.word)"
        >
          <v-list-item-icon>
            <v-icon>mdi-text</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            {{ autoComplete.word }}
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </div>
  </div>
</template>

<script>
const datamuse = require("datamuse");

export default {
  data() {
    return {
      isSearching: false,
      histories: [],
      searchQuery: "",
      autoCompleted: [],
      loading: false,
    };
  },

  mounted() {
    this.histories = this.getSearchedHistory();
  },
  watch: {
    searchQuery(val) {
      this.loading = true;
      datamuse
        .sug({
          s: val,
        })
        .then((json) => {
          console.log(json);
          this.autoCompleted = json;
          this.loading = false;
          //do it!
        });
    },
  },
  methods: {
    setText(val) {
      this.searchQuery = val;
    },

    addToSearchHistory(wordtext) {
      this.setText(wordtext);

      var words = localStorage.getItem("search_query");
      if (words == null) {
        let wordx = [];
        wordx.unshift(wordtext);
        let todb = JSON.stringify(wordx);
        try {
          localStorage.setItem("search_query", todb);
        } catch (err) {}
      } else {
        var res = JSON.parse(words);
        if (res.indexOf(wordtext) != -1) {
          res.splice(res.indexOf(wordtext), 1);
          //  console.log('already in db')
        }
        res.unshift(wordtext);

        let todb = JSON.stringify(res);
        try {
          localStorage.setItem("search_query", todb);
        } catch (err) {}
      }
    },

    getSearchedHistory() {
      var contacts = localStorage.getItem("search_query");
      //console.log(contacts)
      if (contacts == null) {
        return [];
      } else {
        var res = JSON.parse(contacts);
        // var history = [];

        // for (let index = 0; index < res.length; index++) {
        //     const element = res[index];
        //     let nh = this.getContactInfo(element)
        //     if (nh !== undefined) {
        //         history.push(nh)
        //     } else {
        //         this.deleteContact(element)
        //     }
        // }

        return res;
      }
    },
  },
};
</script>

<style scoped>
#wrapperx {
  width: 100vw;
  height: 100vh;
}

.search-result-cover {
  width: 100vw;
  height: calc(100vh - 70px);
  max-width: 600px;
  margin: 0px auto;
  overflow: auto;
}
</style>