<template>
   <section>
       <button @click="setLang('eng')" >ENG</button>
       <button @click="setLang('srb')" >SRB</button>
       <div v-for="(item, index) in this.videos.videos" v-bind:key="index" >
           <img :src="getThumb(item.youtubeId)" alt="">
           <h2>{{ item.title }}</h2>
           <p> {{ item.author }} </p>    
       </div>
   </section>
</template>

<style lang="scss" scoped>
    section {
        max-width: 800px;
        margin: 0 auto;

        div {
            img {
                max-width: 800px;
            }
        }
    }
</style>

<script>
import axios from "axios"

export default {
    name: "videos",
    data() {
        return {
            engApi: "https://api.myjson.com/bins/13mms5",
            srbApi: "https://api.myjson.com/bins/99ixh",
            videos: {},
        }
    },
    methods: {
        getThumb: function(videoId) {
            return youtube.generateThumbnailUrl(videoId);
        },
        setLang: function(lang) {
            agCookie.create("lang", lang, 7);
            if (lang === "eng") {
                 axios.get(this.engApi)
            .then( response => {
            this.videos = response.data;
     })
            } else {
                 axios.get(this.srbApi)
        .then( response => {
         this.videos = response.data;
     })
            }
        },
    },
    // mounted and created will both work here as well
    mounted() {
        var lang = agCookie.read("lang");
       if ( lang != null) {
             if (lang === "eng") {
                 axios.get(this.engApi)
            .then( response => {
            this.videos = response.data;
     })
            } else {
                 axios.get(this.srbApi)
        .then( response => {
         this.videos = response.data;
     })
            }
       } else {
           axios.get(this.engApi)
     .then( response => {
         this.videos = response.data;
     })
       }
    }
}
</script>
