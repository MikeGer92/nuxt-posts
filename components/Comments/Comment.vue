<template>
  <div class="comment-item">
    <div class="comment-item__data">
      <div class="comment-item__data_col post__data_id">{{ comment.postId }}</div>
      <div class="comment-item__data_col post__data_id">{{ comment.id }}</div>
      <div class="comment-item__data_col post__data_title">{{ comment.name }}</div>
      <div class="comment-item__data_col post__data_body">{{ comment.email }}</div>
      <div class="comment-item__data_col post__data_body">{{ comment.body }}</div>
      <nuxt-link :to="'/'" :style="{'textDecoration': 'none', 'display': 'contents'}">
        <div class="comment-item__data_col comment-item__data_btn" ><button type="button" class="comment-item__data_posts" @click="showId(comment.id)">Home</button></div>
      </nuxt-link>
    </div>
  </div>


</template>

<script>
export default {
  name: 'Comment',
  props: {
    comment: {
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
.comment-item {
  display: flex;
  &__data {
    display: flex;
    justify-content: space-around;
    width: 100%;
    border-bottom: 1px solid #000;
    &_col {
      flex-basis: 44%;
      margin: 0 20px 0 40px;
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
        width: 100%;
        height: 40%;
        border-radius: 10%;
       cursor: pointer;
      }
    }
  }
}

</style>
