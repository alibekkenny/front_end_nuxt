<template>
  <div class="">
    <div class="flex flex-wrap justify-center mt-20">
      <div class="w-full max-w-sm">
        <label
          class="block text-indigo-500 text-4xl font-bold text-center mb-6"
        >
          Create user
        </label>
        <div
          v-if="errors.length"
          class="
            bg-red-100
            border border-red-400
            text-red-700
            px-4
            py-3
            rounded
            relative
            mb-4
          "
          role="alert"
        >
          <ul>
            <li class="inline-block" v-for="error in errors" :key="error.id">
              {{ error }}&nbsp;
            </li>
          </ul>
        </div>
        <div class="md:flex md:items-center mb-6">
          <div class="md:w-1/3">
            <label
              class="
                block
                text-gray-500
                font-bold
                md:text-right
                mb-1
                md:mb-0
                pr-4
              "
              for="first_name"
            >
              First Name
            </label>
          </div>
          <div class="md:w-2/3">
            <input
              class="
                bg-gray-200
                appearance-none
                border-2 border-gray-200
                rounded
                w-full
                py-2
                px-4
                text-gray-700
                leading-tight
                focus:outline-none
                focus:bg-white
                focus:border-purple-500
              "
              id="first_name"
              v-model="user.first_name"
              type="text"
            />
          </div>
        </div>
        <div class="md:flex md:items-center mb-6">
          <div class="md:w-1/3">
            <label
              class="
                block
                text-gray-500
                font-bold
                md:text-right
                mb-1
                md:mb-0
                pr-4
              "
              for="salary"
            >
              Salary
            </label>
          </div>
          <div class="md:w-2/3">
            <input
              class="
                bg-gray-200
                appearance-none
                border-2 border-gray-200
                rounded
                w-full
                py-2
                px-4
                text-gray-700
                leading-tight
                focus:outline-none
                focus:bg-white
                focus:border-purple-500
              "
              id="salary"
              v-model="user.salary"
              type="text"
            />
          </div>
        </div>
        <div class="md:flex md:items-center mb-6">
          <div class="md:w-1/3">
            <label
              class="
                block
                text-gray-500
                font-bold
                md:text-right
                mb-1
                md:mb-0
                pr-4
              "
              for="image_path"
            >
              Image path
            </label>
          </div>
          <div class="md:w-2/3">
            <input
              class="
                bg-gray-200
                appearance-none
                border-2 border-gray-200
                rounded
                w-full
                py-2
                px-4
                text-gray-700
                leading-tight
                focus:outline-none
                focus:bg-white
                focus:border-purple-500
              "
              id="image_path"
              v-model="user.image_path"
              type="text"
              placeholder="https://nerdist.com/wp-content/uploads/2020/07/maxresdefault.jpg"
            />
          </div>
        </div>
        <div class="md:flex md:items-center mb-6">
          <div class="md:w-1/3">
            <label
              class="
                block
                text-gray-500
                font-bold
                md:text-right
                mb-1
                md:mb-0
                pr-4
              "
              for="companies"
            >
              Company
            </label>
          </div>
          <div class="md:w-2/3">
            <div class="relative">
              <select
                @change="selectCompanyId()"
                v-model="selected"
                class="
                  block
                  appearance-none
                  w-full
                  bg-gray-200
                  border border-gray-200
                  text-gray-700
                  py-3
                  px-4
                  pr-8
                  rounded
                  leading-tight
                  focus:outline-none
                  focus:bg-white
                  focus:border-gray-500
                "
                id="companies"
              >
                <option
                  v-for="company in companies"
                  :key="company.id"
                  v-bind:value="{ id: company.id }"
                >
                  {{ company.company_name }}
                </option>
              </select>
              <div
                class="
                  pointer-events-none
                  absolute
                  inset-y-0
                  right-0
                  flex
                  items-center
                  px-2
                  text-gray-700
                "
              >
                <svg
                  class="fill-current h-4 w-4"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                >
                  <path
                    d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
                  />
                </svg>
              </div>
            </div>
          </div>
        </div>
        <div class="md:flex md:items-center">
          <div class="md:w-1/3"></div>
          <div class="md:w-2/3">
            <button
              class="
                shadow
                bg-purple-500
                hover:bg-purple-400
                focus:shadow-outline
                focus:outline-none
                text-white
                font-bold
                py-2
                px-4
                rounded
              "
              @click="checkUser()"
            >
              Update
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const app = axios.create({ baseURL: "http://localhost:8080/api/" });

export default {
  data() {
    return {
      user: {},
      errors: [],
      selected: {},
    };
  },
  async mounted() {
    const id = this.$route.params.id;
    this.companies = (await app.get("companies")).data;
    const { data, status } = await app.get("users/" + id);
    this.user = data;
    this.selected.id = this.user.companies_id;
  },
  methods: {
    checkUser() {
      this.errors = [];
      if (this.user.first_name == null) {
        this.errors.push("Wrong first name!");
      }
      if (this.user.salary == null) {
        this.errors.push("Wrong salary!");
      }
      if (this.user.image_path == null) {
        this.errors.push("Wrong image path!");
      }
      if (this.user.companies_id == null) {
        this.errors.push("Wrong company id!");
      }
      if (this.errors.length == 0) {
        this.updateUser();
      }
    },
    updateUser() {
      const updatedUser = {
        first_name: this.user.first_name,
        salary: this.user.salary,
        image_path: this.user.image_path,
        companies_id: this.user.companies_id,
      };
      const { data, status } = app.put("users/" + this.user.id, updatedUser);
      console.log(status);
      console.log(data);
      this.$router.push("/");
    },
    selectCompanyId() {
      this.user.companies_id = Number(this.selected.id);
    },
  },
};
</script>