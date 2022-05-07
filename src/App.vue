<template>
  <div class="app">
    <Header @changeImage="changeImage" />
    <div class="slideshow" id="slideshow">
      <img :src="src" v-show="images.length == 0" />
      <div v-for="(image, id) in images" :key="id">
        <img :src="image" v-show="current == id" />
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Header,
  },
  data() {
    return {
      current: 0,
      src: "https://media.istockphoto.com/photos/golden-retriever-puppy-looking-up-isolated-on-black-backround-picture-id466614709?k=20&m=466614709&s=612x612&w=0&h=liEFwUIRTpkhuvqHMUBRjbFsYa7ign2ihS0d7X07Rxw=",
      images: [],
    };
  },

  methods: {
    async changeImage(breed) {
      if (breed != "Choose a dog breed") {
        const response = await fetch(
          `https://dog.ceo/api/breed/${breed}/images`
        );

        const data = await response.json();
        this.src = data.message[0];
        this.src = data.message[1];
        this.images = data.message;
        const totalImages = data.message.length;
        setInterval(() => {
          if (this.current == totalImages) {
            this.current = 0;
          } else {
            this.current++;
          }
        }, 3000);
      }
    },
  },
};
</script>

<style>
html,
body {
  height: 100%;
}
* {
  margin: 0;
  padding: 0;
}

:root {
  --clr-dark: 230 35% 7%;
  --clr-light: 231 77% 90%;
  --clr-white: 0 0% 100%;

  /* font-families */
  --ff-serif: "Bellefair", serif;
  --ff-sans-cond: "Barlow Condensed", sans-serif;
  --ff-sans-normal: "Barlow", sans-serif;
}

body {
  margin: 0;
  color: hsl(var(--clr-dark));
  font-family: var(--ff-sans-cond);
  line-height: 1.6;
   background: hsl(var(--clr-dark));
}

.app {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
}
.slideshow {
  flex: 1;
  /* height: 700px; */
 
  position: relative;
  overflow: hidden;
}
.slideshow img {
  margin-left: 350px;
   width: 50%;
}

.slide {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center center;
  opacity: 0;
  transform: scale(1);
  transition: all 0.9s ease-out;
}

.slide:nth-last-child(2) {
  opacity: 1;
  transform: scale(1.08);
}
</style>
