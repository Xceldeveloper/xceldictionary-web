<template>
  <div id="wrapper">
    <v-toolbar dark color="rgba(0,0,0,0.3)" absolute width="100%">
      <v-btn icon dark @click="$router.go(-1)">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-toolbar-title
        ><span style="color: rgba(255, 255, 255, 0.3)">{{
          query_word
        }}</span></v-toolbar-title>

      <v-spacer></v-spacer>
      <v-btn text color="rgba(255,255,255,0.5)"
        >search <v-icon right>mdi-magnify</v-icon></v-btn>
    </v-toolbar>
    <v-card dark class="boxer" width="100%" height="250px" color="#000">
      <v-row class="fill-height" align="center" justify="center">
        <v-card color="transparent" elevation="0">
          <span class="main-word">{{ query_word }}</span>
          <v-chip outlined small>adjective</v-chip>
          <v-btn icon><v-icon>mdi-volume-high</v-icon></v-btn>
          <br>
          /{{query_word}}/
        </v-card>
      </v-row>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query_word: "",
    };
  },
  mounted() {
    this.query_word = this.$route.params.word;
  },
};
</script>

<style lang="scss" scoped>
#wrapper {
  width: 100vw;
  height: 100vh;
  overflow: auto;
}

.boxer {
  border-radius: 0px;
  height: 295px;
  width: 100%;
  overflow: auto;
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
  font-size: 35px;
  display: block;
  max-width: 85vw;
}
</style>