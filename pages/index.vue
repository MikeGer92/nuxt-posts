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
    <div class="paginate">
      <div class="paginate__prev" @click="maxPageDown">&lt;</div>
      <div class="paginate__page"
        v-if="page>11"
        :class="{'paginate__page': true, 'paginate__page_active': page === this.startPage }"
        @click="changePage(this.startPage)"
      >
        {{ startPage }}</div>
      <div class="paginate__page_hide" v-if="page>12">...</div>
      <div class="paginate__page"
      v-for="pageNum in pages"
      :key="pageNum"
      :class="{'paginate__page': true, 'paginate__page_active': page === pageNum}"
      @click="changePage(pageNum)"
    >
      {{ pageNum }}</div>
      <div class="paginate__page_hide" v-if="page<39">...</div>
      <div class="paginate__page"
        v-if="page<40"
        :class="{'paginate__page': true, 'paginate__page_active': page === totalPages }"
        @click="changePage(totalPages)"
      >
        {{ totalPages }}</div>
        <div class="paginate__next" @click="maxPageUp">></div>
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
      loading: true,
      page: 1,
      limit: 10,
      totalPages: 0,
      startPage: 1
    }
  },
  computed: {
  pages() {
    let numShown = 21;   // This sets the number of page tabs
    numShown = Math.min(numShown, this.totalPages);
    let first = this.page - Math.floor(numShown / 2);
    first = Math.max(first, 1);
    first = Math.min(first, this.totalPages - numShown + 1);
    return [...Array(numShown)].map((k,i) => i + first);
  }
  },
  mounted() {
      this.getPosts()
  },
  methods: {
    getLoader() {
      this.$nextTick(() => {
        this.$nuxt.$loading.start()
        setTimeout(() => this.$nuxt.$loading.finish(), 500)
      })
    },
    changePage(num) {
      this.page = num
      this.getPosts()
    },
    maxPageUp() {
      if (this.page < this.totalPages ) {
        this.page += 1
      } else {
        this.page= 50
      }
      console.log(this.totalPages)
      console.log(this.page)
      this.getPosts()
    },
    maxPageDown() {
      if (this.page > 1) {
        this.page -= 1
      } else {
        this.page = 1
      }
      this.getPosts()
    },
    async getPosts() {
      await axios.get('https://jsonplaceholder.typicode.com/comments', { params: { _page: this.page, _limit: this.limit } })
        .then(res => {
          this.postsData = res.data
          this.showTable = true
          this.loading = false
          this.totalPages = Math.ceil(res.headers['x-total-count'] / this.limit)
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
  flex-direction: column;
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
.paginate {
  margin: 50px 0;
  display: flex;
  flex-direction: row;
  font-size: 20px;
  flex-wrap: wrap;
  &__page {
    margin: 0 5px;
    width: 35px;
    height: 42px;
    justify-content: center;
    align-items: center;
    border: 1px solid black;
    padding: 5px;
    &_active {
      border: 2px solid teal;
      color: teal;
    }
    &_hide {
      align-self: flex-end;
      margin: 0 5px;
    }
  }
  &__prev, &__next {
    align-self: center;
    font-size: 24px;
    margin: 0 10px;
  }
}
</style>
