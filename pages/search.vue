<template>
  <div id="wrapperx">
    <v-app-bar width="100%" height="60px" dark color="#000" elevation="1" dense>
      <v-btn icon @click="$router.go(-1)">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-spacer></v-spacer>

      <div id="sexc">
        <v-text-field
          hide-details
          autofocus
          elevation="0"
          width="400px"
          v-model="searchQuery"
          prepend-inner-icon="mdi-magnify"
          :append-icon="searchQuery == '' ? 'mdi-microphone' : null"
          @click:append="showMic = true"
          clearable
          single-line
          solo
          :rounded="$vuetify.breakpoint.xs ? false : true"
        ></v-text-field>
      </div>

     
     <v-spacer></v-spacer>

    </v-app-bar>
    <v-progress-linear
      v-if="loading && searchQuery != '' && searchQuery != null"
      height="2px"
      color="#000"
      indeterminate
      width="100%"
    />
    <div class="search-result-cover" v-if="histories.length > 0 || autoCompleted.length > 0">
      <v-list v-if="searchQuery == '' || searchQuery == null  && histories.length > 0">
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

      <v-list v-if="searchQuery != '' || searchQuery != null  && autoCompleted.length > 0">
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
            <span
              :style="{
                'font-weight': autoComplete.word == searchQuery ? 'bold' : null,
              }"
              >{{ autoComplete.word }}</span
            >
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </div>


    <v-bottom-sheet max-width="400px" v-model="showMic">
      <v-card>
        <v-card-text align="center" justify="center">
          <br>
          <v-btn color="#000" fab height="80px" width="80px">
            <v-icon color="#fff" size="50px">mdi-microphone</v-icon>
          </v-btn>
          <br>
           <br>
          <span>Pronouce Word</span>
        </v-card-text>
      </v-card>
    </v-bottom-sheet>
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
      showMic:false
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
  height: auto;
  max-height: calc(100vh - 70px);
  max-width: 750px;
  margin: 5px auto;
  overflow: auto;
  box-shadow: 0px  1px 10px lightgrey;
  border-radius: 10px;
}

#sexc {
  width: calc(100vw - 45px);
  max-width: 700px;
  margin: auto;
}
</style>