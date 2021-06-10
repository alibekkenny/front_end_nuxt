<template>
  <div class="w-full">
    <div v-for="(post, index) in posts" :key="post.id" class="w-full">
      <div
        class="
          border-r border-l border-b border-t border-gray-400
          bg-white
          p-4
          flex flex-col
        "
      >
        <div class="mb-8">
          <div class="text-gray-900 font-bold text-xl mb-2">
            {{ post.title }}
          </div>
          <p class="text-gray-700">
            {{ post.content }}
          </p>
        </div>
        <div class="flex items-center">
          <img
            class="w-10 h-10 rounded-full mr-4"
            :src="user.image_path"
            alt="Avatar of Writer"
          />
          <div class="text-sm">
            <p class="text-gray-900 leading-none">{{ user.first_name }}</p>
            <p class="text-gray-600">{{ user.salary }}</p>
          </div>
        </div>
        <div class="inline-block">
          <div class="block ml-1 float-right">
            <button
              class="
                w-20
                bg-transparent
                hover:bg-green-500
                text-green-700
                font-semibold
                hover:text-white
                py-2
                px-4
                border border-green-500
                hover:border-transparent
                rounded
              "
            >
              Update
            </button>
          </div>
          <div class="block float-right">
            <button
              class="
                w-20
                bg-transparent
                hover:bg-red-500
                text-red-700
                font-semibold
                hover:text-white
                py-2
                px-4
                border border-red-500
                hover:border-transparent
                rounded
              "
              @click="deletePost(index)"
            >
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>

    <Nuxt-Link
      :to="'/posts/create/' + user.id"
      class="
        mt-4
        float-right
        bg-transparent
        hover:bg-blue-500
        text-blue-700
        font-semibold
        hover:text-white
        py-2
        px-4
        border border-blue-500
        hover:border-transparent
        rounded-full
      "
      >Add post</Nuxt-Link
    >
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Posts",
  data() {
    return {
      posts: [],
      user: Object,
    };
  },
  async mounted() {
    const id = this.$route.params.id;
    const { data, status } = await axios.get(
      "http://127.0.0.1:8080/api/users/" + id + "/posts"
    );
    const user = (await axios.get("http://127.0.0.1:8080/api/users/" + id))
      .data;
    console.log(status);
    console.log(user);
    this.posts = data;
    this.user = user;
  },
  methods: {
    deletePost(i) {
      const { data, status } = axios.delete(
        "http://127.0.0.1:8080/api/posts/" + this.posts[i].id
      );
      console.log(status);
      console.log(data);
      this.posts.splice(i, 1);
    },
  },
};
</script>