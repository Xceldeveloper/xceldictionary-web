<template>
  <div id="wrapper">
    <v-row>
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

    <div id="trademark">
      <a href="https://xceldeveloper.com">xceldeveloper</a>
    </div>
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
    this.$axios.$get('https://www.dictionaryapi.com/api/v3/references/collegiate/json/hello?key=0f19822e-171c-47ab-93ee-68209f7117b9').then(res=>{
      console.log(JSON.stringify(res,null,4))
    })
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
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.titlex {
  font-weight: bold;
  width: 100%;
  display: block;
  text-align: center;
  font-size: 25px;
}

#trademark {
  position: fixed;
  bottom: 50px;
  width: 100%;
  left: 0px;
  text-align: center;
}

#trademark a {
  text-decoration: none;
  color: #000;
}
</style>
