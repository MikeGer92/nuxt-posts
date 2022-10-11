<template>
  <div class="container">
    <div>
      <h1 class="title">
        nuxt-posts
      </h1>
      <div class="links">
        <PostsTable
        v-if="!loading && showTable"
        :columns="columns"
        :bodyData="postsData"
        @clickUp="sortByColumnUp"
        @clickDown="sortByColumnDown"
      />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import PostsTable from '@/components/Post/PostsTable.vue'
export default {
  components: {
    PostsTable
  },
  data() {
    return {
      showTable: false,
      columns: ['id', 'name', 'email'],
      postsData: [],
      loading: true
    }
  },
  mounted() {
    // this.$nextTick(() => {
    //   this.$nuxt.$loading.start()
    //   this.loading = true
      this.getPosts()
      // setTimeout(() => this.$nuxt.$loading.finish(), 200)
      // this.loading = false
    // })
  },
  methods: {
    getLoader() {
      this.$nextTick(() => {
      this.$nuxt.$loading.start()
      setTimeout(() => this.$nuxt.$loading.finish(), 500)
    })
    },
    async getPosts() {
      await axios.get('https://jsonplaceholder.typicode.com/comments')
        .then(res => {
          this.postsData = res.data
          this.showTable = true
          this.loading = false
        })
        .catch(err => {
          if (err.response) {
            console.log(err.message)
            alert('Ошибка ответа сервера. Проверьте данные запроса!')
          } else if (err.request) {
            console.log(err)
            alert('Ошибка запроса. Проверьте данные запроса!')
          } else {
            console.log(err)
            alert('Что-то пошло не так. Проверьте данные и попробуйте снова!')
          }
        }
      )

    },
    sortByColumnUp(col) {
      if(col === 'address') {
        this.postsData.sort((a,b) => a.address.city.localeCompare(b.address.city))
      } else if(col === 'username') {
        this.postsData.sort((a,b) => a.username.localeCompare(b.username))
      } else if (col === 'email'){
        this.postsData.sort((a,b) => a.email.localeCompare(b.email))
      } else {
        this.postsData.sort((a,b) => a.id - (b.id))
      }
    },
    sortByColumnDown(col) {
      if(col === 'address') {
        this.postsData.sort((a,b) => b.address.city.localeCompare(a.address.city))
      } else if(col === 'username') {
        this.USERS.sort((a,b) => b.username.localeCompare(a.username))
      } else if (col === 'email'){
        this.postsData.sort((a,b) => b.email.localeCompare(a.email))
      } else {
        this.postsData.sort((a,b) => b.id - (a.id))
      }
    },
  }
}
</script>

<style lang="scss">
[v-cloak] {
    display: none;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
  display: flex;
  justify-content: center;
}
</style>
