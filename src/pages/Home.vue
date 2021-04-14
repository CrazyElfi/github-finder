<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">

        <!-- search-->
        <search
            :value="search"
            placeholder="Type user name ..."
            @search="search = $event"/>

        <button
            class="btn btnPrimary"
            v-if="!repos"
            @click="getRepos">
          Search!
        </button>
        <button
            class="btn btnPrimary"
            v-else
            @click="getRepos">
          Search again!
        </button>
      </div>

      <div class="container">

        <div class="error" v-if="error" style="margin-top: 20px">
          <p>{{ error }}</p>
        </div>
        <div class="repos__wrapper" v-if="repos">
          <div class="repos-item" v-for="repo in repos" :key="repo.id">
            <div class="repos-info">
              <a class="link" target="_blank" :href="repo.html_url">{{ repo.name }}</a>
              <span>{{ repo.stargazers_count }} ⭐ </span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import search from '@/components/Search.vue'
import axios from "axios";

export default {
  components: {search},
  data() {
    return {
      search: '',
      error: null,
      repos: null,
    }
  },
  methods: {
    getRepos() {
      // https://api.github.com/users/vedees/repos
      axios
          .get(`https://api.github.com/users/${this.search}/repos`)
          .then(res => {
            console.log('1', res)
            this.error = null
            this.repos = res.data
          })
          .catch(error => {
            console.log(error)
            this.repos = null
            this.error = "Can't find this user"
          })
    }
  }
}
</script>
<style lang="scss" scoped>
.container {
  display: flex;
  align-items: center;
  //flex-direction: column;
  justify-content: center;
  //margin-bottom: 40px;
}

button {
  //margin-top: 40px;
  margin-left: 40px;
}

.repos__wrapper {
  width: 600px;
  margin: 30px 0;
}

.repos-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px 0;
  border-bottom: 1px solid #dbdbdb;
}
</style>
