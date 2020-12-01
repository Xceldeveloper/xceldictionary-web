<template>
  <v-app >
       <nuxt />
  </v-app>
</template>

<script>
export default {
  data () {
    return {
     
    }
  },
    mounted(){
    //polifix for persisent scrollbar
   let eHtml = document.getElementsByTagName('html')[0]
   eHtml.style.overflowY = 'hidden'
  },
}
</script>
