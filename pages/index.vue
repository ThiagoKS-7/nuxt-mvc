<template>
  <div>
    <Header/>
    <div class="grid grid-cols-2 md:grid-cols-3">
        <div v-for="news in newsList" class="inactive-card" @click="checkDetails(news.id)">
          <img :src="getImageUrl(news.id)" class="rounded-[1em]"/>
          <div class="flex flex-col ml-9">
            <h1 class="text-md md:text-3xl"> News #{{ news.id }}: </h1>
            <h3 class="text-sm md:text-xl">Name: {{news.name }}</h3>
            <h3 class="text-sm md:text-sm">Description: {{news.description }}</h3>
          </div>
        </div>
    </div>  
    <!-- <div class="flex justify-end">
      <div class="mx-3"  v-if="newsList?.pagination?.links?.prev != null">
        <span class="pagination-link" @click="getnewssList(`/admin/house_newss?page=${newsList.pagination.current_page -1}`)">Previous</span>
      </div>
      <div class="mx-[2em]" v-if="newsList?.pagination?.links?.next != null">
        <span class="pagination-link" @click="getnewssList(`/admin/house_newss?page=${newsList.pagination.current_page +1}`)">Next</span>
      </div>
    </div> -->
  </div>
</template>

<script>

export default {
  name: 'IndexPage',
  data() {
    return {
      newsList: []
    }
  }, 
  mounted() {
    this.getNewsList("/news");
  },
  methods: {
    async getNewsList(path) {
      try {
        const token = window.localStorage.getItem("token")
        if (token) {
          this.$axios.setToken(token, 'Bearer');
          this.newsList  = await this.$axios.$get(path)
        }
      } catch (e) {
        console.error(e)
      }
    },
    getImageUrl(data) {
      return `https://picsum.photos/200/?id=${data}`
    },
    checkDetails(data) {
      this.$router.push(`/details/${data}`)
    }
  }
 }
</script>
