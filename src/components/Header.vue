<template>
  <div class="header">
    <h1>Infinite Dog App</h1>
    <div id="breed">
      <select @change="onChange">
        <option value="">Choose a dog breed</option>
        <option :value="breed" v-for="(_, breed) in breeds" :key="breed">
          {{ breed }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  emits: ["changeImage"],
  data() {
    return {
      breeds: {},
      timer: "",
      nextSlide: "",
      deleteFirstPhotoDelay: "",
    };
  },
  created() {
    this.getBreeds();
  },
  methods: {
    onChange(event) {
      this.$emit("changeImage", event.target.value);
    },
    getBreeds() {
      fetch("https://dog.ceo/api/breeds/list/all")
        .then((res) => res.json())
        .then((data) => {
          this.breeds = data.message;
        });
    },
  },
};
</script>

<style scoped>
.header {
    text-align: center;
    padding: 20px 0;
   background: hsl(var(--clr-light));
  }
  
  .header h1 {
    margin: 0 0 10px 0;
   font-family: var(--ff-sans-cond);
   font-size: 32px;
  }
  
  .header select {
    font-size: 1.6rem;
    font-family: var(--ff-serif);
    height: 40px;
    width: 20%;
    text-align: center;
    border: 3px solid hsl(var(--clr-dark));
  
  }
 
 
</style>
