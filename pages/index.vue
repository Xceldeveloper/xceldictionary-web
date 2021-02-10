<template>
  <div id="wrapper">
    <v-row class="fill-height ma-0" align="center" justify="center">
      <div elevation="0" class="coverx">
        <span class="titlex">XcelDictionary</span>
        <br />
        <br />
        <v-text-field
          @focus="$router.push('/search')"
          outlined
          rounded
          placeholder="type in word..."
          append-icon="mdi-magnify"
        ></v-text-field>
      </div>
    </v-row>
  </div>
</template>

<script>
export default {
  mounted() {
    // var Owlbot = require('owlbot-js');
    // var client = Owlbot('99ced7e7be855d4d3648ce60d37195c8cb27ea04');
    // client.define('boss').then(function(result){
    //    console.log(JSON.stringify(result,null,2));
    // });
    // this.$axios.$get('https://www.dictionaryapi.com/api/v3/references/collegiate/json/boy?key=0f19822e-171c-47ab-93ee-68209f7117b9').then(res=>{
    //   console.log(JSON.stringify(res,null,4))
    // })
  },
};
</script>

<style lang="scss" scoped>
#wrapper {
  width: 100vw;
  height: 100vh;
}

.coverx {
  width: 90%;
  max-width: 600px;
}

.titlex {
  font-weight: bold;
  width: 100%;
  display: block;
  text-align: center;
  font-size: 25px;
}
</style>
