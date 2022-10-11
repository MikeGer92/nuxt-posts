<template>
  <div class="post">
    <div class="post__data">
      <div class="post__data_col post__data_id">{{ post.id }}</div>
      <div class="post__data_col post__data_title">{{ post.name }}</div>
      <div class="post__data_col post__data_body">{{ post.email }}</div>
      <nuxt-link :to="`/comments/${post.id}`" :style="{'textDecoration': 'none', 'display': 'contents'}">
        <div class="post__data_col post__data_btn" ><button type="button" class="post__data_posts" @click="showId(post.id)">Detail</button></div>
      </nuxt-link>
    </div>
  </div>


</template>

<script>
export default {
  name: 'Post',
  props: {
    post: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
    }
  },
  methods: {
    showId(id) {
      console.log(id)
    }
  }
}
</script>

<style lang="scss">
.post {
  display: flex;
  &__data {
    display: flex;
    justify-content: space-around;
    width: 100%;
    height: 120px;
    border-bottom: 1px solid #000;
    &_col {
      flex-basis: 44%;
      margin: 0 20px;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      height: 100%;
      font-size: 20px;
      &:first-child {
        flex-basis: 5%;
      }
      &:last-child {
        flex-basis: 7%;
      }
    }
    &_btn {
      & button {
        width: 70%;
        height: 40%;
        border-radius: 10%;
       cursor: pointer;
      }
    }
  }
}

</style>
