<template>
  <div id="wrapper">
    <v-toolbar color="#fff" elevation="0" absolute width="100%">
      <v-btn icon @click="$router.go(-1)">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-toolbar-title
        ><span style="color: rgba(0, 0, 0, 0.3)">{{
          query_word
        }}</span></v-toolbar-title
      >

      <v-spacer></v-spacer>
      <v-btn text color="rgba(0,0,0,0.5)" to="/search">
        <v-icon>mdi-magnify</v-icon></v-btn
      >
    </v-toolbar>
    <v-card class="boxer">
      <v-row class="fill-height ma-0" align="center" justify="center">
        <v-card
          color="transparent"
          elevation="0"
          align="center"
          justify="center"
        >
          <span class="main-word">{{ query_word }}</span>
          /{{ pronouciation }}/
          <!-- <v-btn icon light right><v-icon>mdi-volume-high</v-icon></v-btn> -->
        </v-card>
      </v-row>
    </v-card>

    <div class="word-wraps">
      <v-card class="cont" v-for="(def, index) in defs" :key="index">
        <v-card-title
          ><v-chip outlined>{{ def.type }}</v-chip></v-card-title
        >
        <v-card-text>
          {{ def.definition }}
          <span v-if="def.example != null">
            <br /><br />
            <v-divider></v-divider>
            <br />
            <b>Example :</b>{{ def.example }}
          </span>
        </v-card-text>
      </v-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query_word: "",
      type: "",
      pronouciation: "",
      defs: [],
    };
  },
  mounted() {
    this.query_word = this.$route.params.word;
    this.getWord();
  },

  methods: {
    getWord() {
      var Owlbot = require("owlbot-js");
      var client = Owlbot("99ced7e7be855d4d3648ce60d37195c8cb27ea04");
      client.define(this.query_word).then((result) => {
        console.log(JSON.stringify(result, null, 2));
        this.pronouciation = result.pronunciation;
        this.defs = result.definitions;
      });
    },
  },
};
</script>

<style lang="scss" scoped>
#wrapper {
  width: 100vw;
  height: 100vh;
  overflow: auto;
  background-color: #000;
}

.boxer {
  background-color: #fff;
  border-radius: 0px;
  height: 320px;
  width: auto;

  clip-path: polygon(
    50% 0%,
    89% 0,
    100% 0,
    100% 70%,
    80% 90%,
    50% 100%,
    20% 90%,
    0% 70%,
    0% 35%,
    0 0
  );
}

.main-word {
  font-size: 30px;
  display: block;
  max-width: 85vw;
  word-break: normal;
}

.cont {
  margin: 10px auto;
}

.word-wraps {
  width: 100%;
  max-width: 500px;
  margin: auto;
  padding: 10px;
  display: block;
}
</style>