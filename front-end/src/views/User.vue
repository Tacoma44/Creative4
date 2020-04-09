<template>
<div class="admin">
  <h1>Welcome User!</h1>
  <div>
    <div class="heading">
      <div class="circle">1</div>
      <h2>Choose a Picture (Will Not Save Until Upload Is Hit)</h2>
    </div>
    <div class="add">
      <div class="form">
        <!-- <input v-model="location" placeholder="Location">
        <input v-model="description" placeholder="User Update"> -->
        <p></p>
        <input type="file" name="photo" @change="fileChanged">
        <button class='new' @click="view">View</button>
        <!-- <button class='new' @click="upload">Upload</button> -->
      </div>
      <!-- <div class="upload" v-if="addItem"> -->
      <div class="upload" v-if="views">

        <!-- <h2>Item Location: {{addItem.location}}</h2>
        <h2>Item Update: {{addItem.description}}</h2> -->
        <p class='yay'>Excelent Choice!</p>
        <img :src="views.path" />
      </div>
    </div>
    <div class="heading">
      <div class="circle">2</div>
      <h2>Share An Update!</h2>
    </div>
    <div class="info">
      <div class='detial'>
        <input v-model="location" placeholder="Location">
        <input v-model="description" placeholder="User Update">
        <button class='new' @click="grab">Combine</button>
      </div>
      <div class='image2' v-if="info">
        <div class='title'>
          <h2>Location: {{info.location}}</h2>
        </div>
        <div class='pic'>
          <img :src="views.path" width="300px" height="200px" />
        </div>
        <div class='update'>
          <h2>Post: {{info.description}}</h2>
        </div>
      </div>
    </div>
    <div class="heading">
      <div class="circle">3</div>
      <h2>Don't Forget To Upload!</h2>
    </div>
      <button class='new' @click="upload">Upload</button>
      <div v-if="complete">
        <h2>{{this.complete}}</h2>
      </div>
  </div>
    <div class="heading">
      <div class="circle">4</div>
      <h2>Edit/Delete a Post</h2>
    </div>
    <div class="edit">
      <div class="form">
        <input v-model="findTitle" placeholder="Search">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.location}}
            <!-- <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.location}} -->
          </div>
        </div>
      </div>
      <div class="upload" v-if="findItem">
        <p><strong>Update Location:</strong></p>
        <input v-model="findItem.location" placeholder="Update Location">
        <p></p>
        <p><strong>Update Post:</strong></p>
        <input v-model="findItem.description" placeholder="Update Post">
        <p></p>
        <p><strong>Update Picture:</strong></p>
          <input type="file" name="photo" @change="fileChanged">
        <p></p>
        <img :src="findItem.path" />
      </div>
      <div class="actions" v-if="findItem">
        <button @click="deleteItem(findItem)">Delete</button>
        <button @click="editItem(findItem)">Edit</button>
      </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'User',
  data() {
    return {
      location: "",
      description: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
      views: null,
      info: null,
      complete: ""
    }
  },
  computed: {
    suggestions() {
      let items = this.items.filter(item => item.location.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.location > b.location);
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
        try {
          const formData = new FormData();
          formData.append('photo', this.file, this.file.name)
          let r1 = await axios.post('/api/photos', formData);
          let r2 = await axios.post('/api/items', {
            location: this.location,
            description: this.description,
            path: r1.data.path
          });
          this.addItem = r2.data;
          this.complete = "Uploaded!";
        } catch (error) {
          this.complete = "Not Uploaded";
          //console.log(error);
        }
      },
    async view() {
      const formData = new FormData();
      formData.append('photo', this.file, this.file.name)
      let r = await axios.post('/api/photos', formData);
      this.views = r.data;
    },
    grab(){
      // const formData = new FormData();
      // formData.append(this.location, this.description)
      let r4 = {
        location: this.location,
        description: this.description
      };
      this.info = r4;
    },
  async getItems() {
    try {
      let response = await axios.get("/api/items");
      this.items = response.data;
      return true;
    } catch (error) {
      //console.log(error);
    }
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
      //  console.log(error);
      }
    },
    async editItem(item) {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r5 = await axios.post("/api/photos/", formData);
        await axios.put("/api/items/" + item._id, {
          location: this.findItem.location,
          description: this.findItem.description,
          path: r5.data.path,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
      //v  console.log(error);
      }
    },
  },
}
</script>

<style scoped>
.admin {

}

.image h2 {
  font-style: italic;
  font-size: 1em;
}

.info {
  margin-top: 30px;
  margin-bottom: 30px;
}

.heading {
  display: flex;
  margin-bottom: 30px;
  margin-top: 30px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.info,
.add,
.edit {
  display: flex;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

.new {
  margin-left: 10px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  text-align: center;
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}

.yay {
  text-align: center;
}

.detial{
  margin-top: 20px;
}

.image2 {
  margin-left: 50px;

  width: 300px;
  border: 1px solid grey;
  border-radius: 25px;
  height: inherit;
  text-align: center;
}
</style>
