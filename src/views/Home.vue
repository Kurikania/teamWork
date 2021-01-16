<template>
  <div class="home">
    <HelloWorld msg="Todo Team Work!" />
    <button @click="sort">   Сортировать по важности </button>
    <div class="create-post">
      <label for="create-post">New Todo</label>
      <input
        type="text"
        id="create-post"
        v-model="text"
        placeholder="Enter new todo"
      />
      <label for="create-post">Очередность выполнения</label>
      <select name="" id="" v-model="range">
        <option v-for="n in posts.length" :key="n" :value="n">{{n}}</option>
      </select>
      <button v-on:click="createPost">Add Todo</button>
      <div class="container"> 
      <div class="posts-container" v-if="!rangeTime">
        <div
          class="post pending"
          v-for="post in posts"
          :item="post"
          :key="post.id"
          :class="{ done: post.isDone }"
        >
          <img id="delete-btn" @click="deleteTodo(post.id)" src="../assets/del.png" alt=""> 

          <p class="text">Очередность выполнения: {{ post.range }}</p>
          <p class="text">{{ post.text }}</p>
          <p>Время создания: {{ post.date }}</p>

          <button class="done-btn" @click="done(post.id)" v-if="post.isDone">Не сделано</button>
          <button class="undone-btn" @click="done(post.id)" v-if="!post.isDone">Сделано</button>
        </div>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
  data() {
    return {
      rangeTime: false,
      text: "",
      posts: [],
      range: null
    };
  },
  watch: {
    // posts(newPost, oldPost) {
    //   console.log("Watch")
    // }
  },
  methods: {
    createPost() {
      let date = new Date();
      this.posts.push({
        text: this.text,
        id: Date.now(),
        isDone: false,
        range: this.range,
        date:
          (date.getUTCHours() + 3 < 10
            ? "0" + (date.getUTCHours() + 3)
            : date.getUTCHours() + 3) +
          ":" +
          (date.getUTCMinutes() < 10
            ? "0" + date.getUTCMinutes()
            : date.getUTCMinutes()) +
          "\n" +
          date.getUTCDate() +
          "." +
          (date.getUTCMonth() + 1) +
          "." +
          date.getUTCFullYear(),
      }, 
     );
      this.text = "";
      this.saveLocal()
    },
    deleteTodo(id) {
      this.posts = this.posts.filter((a) => a.id !== id);
      this.saveLocal()
    },
    done(id) {
      if (this.posts.find((a) => a.id === id).isDone) {
        this.posts.find((a) => a.id === id).isDone = false;
      } else {
        this.posts.find((a) => a.id === id).isDone = true;
      }
      this.saveLocal()
    },
    saveLocal() {
      localStorage.setItem('toods', JSON.stringify(this.posts))
    }, 
    sort() {
      this.posts= this.posts.sort(function(a,b){return a.range - b.range })
      
    }
  },
  mounted() {    
    this.posts = JSON.parse(localStorage.getItem('toods'))
  }
};
</script>

<style scoped>
div.container {
  max-width: 800px;
  margin: 0 auto;
}

div.post {
  position: relative;
  padding: 10px 10px 30px 10px;
  margin-bottom: 15px;
}
#delete-btn { 
  font-weight: bold;
  border: none;
  width: 30px;
  position: absolute;
  left: 95%;
}
.done {
  border: 1px solid #5bd658 !important;
  background-color: #bcffb8 !important;
}
.pending {
  border: 1px solid #3f3f3f;
  background-color: #e4e4e4;
}
div.created-at {
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15px 5px 15px;
  background-color: darkgreen;
  color: white;
  font-size: 13px;
}
p.text {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 0;
}
</style>