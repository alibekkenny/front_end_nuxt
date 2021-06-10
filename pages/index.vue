<template>
  <div class="">
    <div class="block">
      <Nuxt-Link
        to="/users/create"
        class="
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
      >
        +
      </Nuxt-Link>
    </div>
    <table class="rounded-t-lg m-5 w-5/6 mx-auto bg-gray-200 text-gray-800">
      <tr class="text-left border-b-2 border-gray-300">
        <th class="px-4 py-3">Image</th>
        <th class="px-4 py-3">ID</th>
        <th class="px-4 py-3">Firstname</th>
        <th class="px-4 py-3">Salary</th>
        <th class="px-4 py-3">Companies_id</th>
        <th class="px-4 py-3">Actions</th>
      </tr>

      <tr
        v-for="(user, index) in users"
        :key="user.id"
        class="bg-gray-100 border-b border-gray-200"
      >
        <td class="px-4 py-3">
          <img class="rounded-full h-32 w-32" :src="user.image_path" alt="" />
        </td>
        <td class="px-4 py-3">{{ user.id }}</td>
        <td class="px-4 py-3">{{ user.first_name }}</td>
        <td class="px-4 py-3">{{ user.salary }}</td>
        <td class="px-4 py-3">{{ user.companies_id }}</td>
        <td class="px-4 py-3">
          <div class="block">
            <button
              @click="deleteUser(index)"
              class="
                inline
                px-4
                py-3
                bg-red-500
                hover:bg-red-700
                text-white
                rounded
              "
            >
              Delete
            </button>
            <Nuxt-Link
              :to="'/users/update/' + user.id"
              class="
                inline
                px-4
                py-3
                bg-pink-500
                hover:bg-red-700
                text-white
                rounded
              "
            >
              Update</Nuxt-Link
            >
          </div>
        </td>
        <td class="px-4 py-3">
          <Nuxt-Link
            class="px-4 py-3 bg-blue-500 hover:bg-blue-700 text-white rounded"
            :to="'posts/' + user.id"
            >Messages
          </Nuxt-Link>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from "axios";
const app = axios.create({ baseURL: "http://localhost:8080/api/" });

export default {
  data() {
    return {
      users: [],
    };
  },
  async mounted() {
    const { data, status } = await app.get("users");
    console.log(status);
    this.users = data;
  },
  methods: {
    deleteUser(i) {
      const { data, status } = app.delete("users/" + this.users[i].id);
      console.log(status);
      console.log(data);
      this.users.splice(i, 1);
    },
  },
};
</script>

<style scoped>
</style>
