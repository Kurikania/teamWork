<template>
  <div class="home">
    <HelloWorld msg="Team Work!" />
    <div class="create-post">
      <label for="create-post">Say smth</label>
      <input
        type="text"
        id="create-post"
        v-model="text"
        placeholder="Enter new post"
      />
      <button v-on:click="createPost">Add Todo</button>
      <div class="posts-container">
        <div
          class="post pending"
          v-for="post in posts"
          :item="post"
          :key="post.id"
          :class="{ done: post.isDone }"
        >
        <button @click="done(post.id)">Сделано</button>
        
          <p class="text">{{ post.text }}</p>

          <button @click="deleteTodo(post.id)">Удалить</button>
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
      text: "",
      posts: [],
    };
  },
  methods: {
    createPost() {
      this.posts.push({ text: this.text, id: Date.now(), isDone: false });
    },
    deleteTodo(id) {
      this.posts = this.posts.filter((a) => a.id !== id);
    },
    done(id) {
      console.log(this.posts.filter((a) => a.id !== id).isDone)
      this.posts.find((a) => a.id === id).isDone = true;
    },
  },
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