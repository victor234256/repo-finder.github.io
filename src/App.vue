<!-- src/App.vue -->
<template>
  <div class="page">
    <div class="landing-page-container">
      <div class="left-side">
        <form class="form" @submit.prevent="handleSubmit">
          <input
            class="input"
            v-model="username"
            placeholder="Github Username"
          />
          <button class="button" type="submit">
            {{ loading ? "Searching..." : "Search" }}
          </button>
        </form>
        <div class="results-container">
          <div
            class="row"
            v-for="repo in repos"
            :key="repo.id"
            @click="getDetails(repo.name)"
          >
            <h2 class="repo-name">{{ repo.name }}</h2>
          </div>
        </div>
      </div>
      <MyRepoDetails :details="details" :loading="detailsLoading" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import MyRepoDetails from "./components/MyRepoDetails.vue";

export default {
  components: {
    MyRepoDetails,
  },
  data() {
    return {
      username: "",
      loading: false,
      repos: [],
      details: {},
      detailsLoading: false,
    };
  },
  methods: {
    handleSubmit() {
      this.searchRepos();
    },
    searchRepos() {
      this.loading = true;
      axios
        .get(`https://api.github.com/users/${this.username}/repos`)
        .then((res) => {
          this.loading = false;
          this.repos = res.data;
        })
        .catch((error) => {
          console.error("Error fetching repositories:", error);
          this.loading = false;
          this.repos = [];
        });
    },
    getDetails(repoName) {
      this.detailsLoading = true;
      axios
        .get(`https://api.github.com/repos/${this.username}/${repoName}`)
        .then((res) => {
          this.detailsLoading = false;
          this.details = res.data;
        });
    },
  },
};
</script>

<style>
.page {
  height: 100vh;
  width: 100%;
}

.landing-page-container {
  height: 100%;
  min-height: 100%;
  width: 100%;
  display: flex;
  background-color: #282d35;
  overflow-y: scroll;
}

.left-side {
  height: 100%;
  width: 75%;
  display: flex;
  flex-direction: column;
  border-right: 5px solid rgb(77, 248, 9);
}

.form {
  width: 100%;
  height: 40px;
  display: flex;
  justify-content: center;
  margin-top: 15px;
}

.input {
  width: 30%;
  height: 40px;
  border: none;
  border-radius: 6px;
  font-size: 18px;
  padding-left: 10px;
  background-color: rgba(255, 255, 255, 0.3);
  color: rgb(77, 248, 9);
}

.input:focus {
  border: 1px solid rgb(77, 248, 9);
  outline: none;
}

.input::placeholder {
  color: #282d35;
}

.button {
  height: 100%;
  width: 10%;
  background-color: rgb(77, 248, 9);
  border-radius: 5px;
  border: none;
  font-size: 18px;
  color: #282d35;
  box-sizing: content-box;
  margin-left: 10px;
  cursor: pointer;
}

.button:focus {
  outline: none;
}

.results-container {
  margin-top: 10px;
  height: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.row {
  display: flex;
  cursor: pointer;
  width: 44%;
  height: 45px;
  margin-top: 10px;
  background-color: transparent;
  border-radius: 5px;
  align-items: center;
  justify-content: flex-start;
  pointer-events: none;
}

.row > h2 {
  color: rgb(77, 248, 9);
  pointer-events: auto;
}

.row:hover {
  background-color: rgb(77, 248, 9);
}

.row > h2:hover {
  color: #e50c0c;
}

.repo-name {
  margin-left: 5px;
}
</style>
