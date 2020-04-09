<template>
  <div class="home">
    <section class="image-gallery">
      <div class="image" v-for="item in items" :key="item.id">
        <div class='title'>
          <h2>{{item.location}}</h2>
        </div>
        <div class='pic'>
          <img :src="item.path" />
        </div>
        <div class='update'>
          <h2>{{item.description}}</h2>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'Home',
  data() {
    return {
     items: [],
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>

<style scoped>
.image h2 {
  font-style: italic;
}

/* Masonry */
*,
*:before,
*:after {
  /*box-sizing: inherit;*/

}

.image-gallery {
  column-gap: 1.5em;
  flex-wrap: wrap;
  display: flex;
  justify-content: center;

}

.image {
  margin: 0 0 1.5em;

  width: 400px;
  border: 1px solid grey;
  border-radius: 25px;
  height: inherit;
  text-align: center;
}

.image img {
  width: 100%;
}

.title {
  margin-top: 10px;
}

.pic {
  border-top: 1px solid black;

}

/* Masonry on large screens */
@media only screen and (min-width: 1024px) {
  .image-gallery {
    column-count: 4;

  }
}

/* Masonry on medium-sized screens */
@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 3;
  }
}

/* Masonry on small screens */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 2;
  }
}
</style>
