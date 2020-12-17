<template>
  <div class="home">
    <section class="image-gallery">
      <div class="image" v-for="item in items" :key="item.id">
        <div class="mail">
          <h2>{{ item.title }}</h2>
          <div class="email-box">
            <p>{{ item.description }}</p>
          </div>

          <!-- comment -->
          <div class="commentContainer">
          <div class="comment">
            <h3>comments:</h3>
            <h3 v-for="index in item.comment.length" :key="index"><div class="singleComment"><p>{{item.comment[index-1]}}</p></div></h3>
          </div>
          </div>

          <button class="button" @click="deleteItem(item)">Delete Message</button>
          <button class="button" id="button" @click="addPrompt()">Add Progress Comment</button>

          <div id="commentPrompt">
            <textarea class="form-box" v-model="comment" cols="30" rows="7"></textarea>
            <button class="button" id="submitButton" @click="uploadPrompt(item)">Post Comment</button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      items: [],
      comment: "",
    };
  },
  created() {
    this.getItems();
  },
  computed: {
    outputComment(comments) {
      let output = `<h3>`;
      for (let i = 0; i < comments.length; i++) {
        output += comments[i];
        output += `<br/>`;
      }
      output += `</h3>`;

      return output;
    },
    suggestions() {
      let items = this.items.filter((item) =>
        item.title.toLowerCase().startsWith(this.findTitle.toLowerCase())
      );
      return items.sort((a, b) => a.title > b.title);
    },
  },
  methods: {
    async addPrompt() {
      if (document.getElementById("commentPrompt").style.display == "none") {
        //alert("hi");
        document.getElementById("commentPrompt").style.display = "block";
        document.getElementById("button").innerHTML = "Close Comment Box";
      } else {
        document.getElementById("commentPrompt").style.display = "none";
        document.getElementById("button").innerHTML = "Make a Comment";
      }
    },

    async uploadPrompt(item) {
      console.log(item._id);
      // this.item.comment.push(this.comment);
      await axios.post("/api/comment/" + item._id, {
        comment: this.comment,
      });
      alert("Successfully submitted");
    },

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
    /*async addComment(item) {
      try {
        await axios.put("/api/items/" + item._id {
          title: this.findItem.title,
          description: this.findItem.description,
        });
      } catch(error) {
        console.log(error);
      }
    },*/
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
  },
};
</script>

<style scoped>
h3 {
  color:#f48051;
  font-size: 15px;
}

.image h2 {
}

.commentContainer {
  display: flex;
  justify-content: right;
  flex-wrap: wrap;
  width: 100%;
}

.comment {
  display: block;
  width: 100%;
}

.singleComment {
  padding-right: 5px;
  background-color: #f48051;
  max-width: 40%;
  color: white;
  padding: 1px 5px 1px 10px;
  border-radius: 5px;
}



#commentPrompt {
  display: none;
  border: none;
  outline: none;
}
/* Masonry */
*,
*:before,
*:after {
  box-sizing: inherit;
}

.button {
  background-color: #4d4d4d;
  margin-top: 0px;
}

.home {
  max-width: 400px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 200px;
  margin-right: auto;
  margin-left: auto;
  flex-direction: column;
}

.image-gallery {
  column-gap: 1.5em;
  margin: 5px auto 5px auto;
  display: flex;
  flex-wrap: wrap;
  max-width: 700px;
  min-height: 500px;
}

.mail {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.image {
  margin: 0 auto 1.5em auto;
  width: 100%;
}

.email-box {
  background-color: #4d4d4d;
  color: white;
  padding: 4px 4px 4px 20px;
  border-radius: 5px;
  width: 100%;
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

/* Masonry on small screens  */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 2;
  }
}
</style>