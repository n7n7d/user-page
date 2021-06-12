<template>
  <div class="columns mt-5 is-centered">
    <div class="column is-7">
      <h1 class="title">User List</h1>

      <div class="box">
        <ul>
          <li v-for="item in givePage()" :key="item.id">
            <img :src="item.avatar" alt="" /><br />
            <strong>Email:</strong> {{ item.email }}<br />
            <strong>First Name:</strong> {{ item.first_name }} <br />
            <strong>Last Name:</strong> {{ item.last_name }}
            <hr />
          </li>
        </ul>

        <div class="field has-addons">
          <p class="control">
            <button class="button" @click="isActive(1)">
              <span>1</span>
            </button>
          </p>
          <p class="control">
            <button class="button" @click="isActive(2)">
              <span>2</span>
            </button>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "User List",
  data() {
    return {
      page1: [],
      page2: [],
      active: 2,
    };
  },
  mounted() {
    this.page1 = [];
    this.page2 = [];
    let page1url = "https://reqres.in/api/users?page=1";
    let page2url = "https://reqres.in/api/users?page=2";

    const req1 = axios.get(page1url);
    const req2 = axios.get(page2url);

    axios
      .all([req1, req2])
      .then(
        axios.spread((...responses) => {
          this.page1 = responses[0].data.data;
          this.page2 = responses[1].data.data;
          // use/access the results
        })
      )
      .catch((error) => {
        // react on errors.
        console.error(error);
      });
  },
  methods: {
    givePage() {
      if (this.active == 1) {
        return this.page1;
      }
      if (this.active == 2) {
        return this.page2;
      }
    },
    isActive(x) {
      if (x == 1) {
        this.active = 1;
      }
      if (x == 2) {
        this.active = 2;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
</style>