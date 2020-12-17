<template>
  <div class="sendMessage">
    <h1 class="letschat">Send us a message</h1>
    <div class="heading">
      <h2>What's your email?</h2>
    </div>
    <div class="add">
      <div class="form">
        <input class="form-box" v-model="title" placeholder="  email"/>
        <br />
        <br />
        <h2>How can we help?</h2>
        <textarea class="form-box" v-model="description" cols="30" rows="7"></textarea>
        <button class="button" @click="upload">Send</button>
      </div>
      <div class="upload" v-if="addItem">
        <h2>{{addItem.title}}</h2>
        <img :src="addItem.path" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Admin",
  data() {
    return {
      title: "",
      description: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
    };
  },
  created() {
    this.getItems();
  },
  computed: {
    suggestions() {
      let items = this.items.filter((item) =>
        item.title.toLowerCase().startsWith(this.findTitle.toLowerCase())
      );
      return items.sort((a, b) => a.title > b.title);
    },
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
    fileChanged(event) {
      this.file = event.target.files[0];
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          description: this.findItem.description,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async upload() {
      try {
        let r2 = await axios.post("/api/items", {
          title: this.title,
          description: this.description,
        });
        this.addItem = r2.data;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>


<style scoped>
h2 {
  text-align: center;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
  justify-content: center;
}

.add,
.edit {
  display: flex;
}

.letschat {
  text-align: center;
  font-size: 50px;
  color: #4D4D4D;
}


/* Form <button @click="upload">Upload</button> */


input,
textarea,
select,
button {
  font-family: "Montserrat", sans-serif;
  font-size: 1em;
}

.form {
  margin: 0px auto 0px auto;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
}

.form-box {
  border-radius: 10px;
  border-width: 1px;
}

.button {
  width: 70%;
  margin: 30px auto 10px auto;
  background-color: #f48051;
  border: none;
  color: white;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  font-size: 20px;
  padding: 10px 0px 10px 0px;
}

.sendMessage {
  margin-bottom: 150px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
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
  background-color: #5bdeff;
  color: #fff;
}
</style>
