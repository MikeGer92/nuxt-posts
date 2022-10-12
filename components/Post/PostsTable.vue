<template>
  <div class="posts-table">
    <div class="posts-table__header">
      <div class="posts-table__header_col" v-for="col in columns" :key="col">{{ capitalize(col) }}
        <div class="posts-table__header_col-img">
          <div v-if="sortUp" class="posts-table__header_col-img--up" @click="sendClickUp(col)"><img :src="require(`@/assets/images/sortActive.png`)" /></div>
          <div v-else class="posts-table__header_col-img--up" @click="sendClickUp(col)"><img :src="require(`@/assets/images/sortDown.png`)" /></div>
          <div v-if="sortDown" class="posts-table__header_col-img--down" @click="sendClickDown(col)"><img :src="require(`@/assets/images/sortActive.png`)" /></div>
          <div v-else class="posts-table__header_col-img--down" @click="sendClickDown(col)"><img :src="require(`@/assets/images/sortDown.png`)" /></div>
        </div>
      </div>
    </div>
    <div class="posts-table__body" v-for="item in bodyData" :key="item.id">
      <Post :post="item"/>
    </div>

  </div>
</template>

<script>
import Post from '@/components/Post/Post.vue'
export default {
  name: 'PostsTable',
  components: { Post },
  props: {
    columns: {
      type: Array,
      default: () => []
    },
    bodyData: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      sortUp: true,
      sortDown: false,
    }
  },
  methods: {
    capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    },
    sendClickUp(col) {
      this.sortUp = true
      this.sortDown = false
      this.$emit('clickUp', col)
    },
    sendClickDown(col) {
      this.sortUp = false
      this.sortDown = true
      this.$emit('clickDown', col)
    },
  }

}
</script>

<style lang="scss">
.posts-table {
  display: flex;
  flex-direction: column;
  width: 90%;
  max-width: 1600px;
  border: 2px solid #8e7bea;
  &__header {
    display: flex;
    width: 100%;
    height: 70px;
    background: #8e7bea;
    border-bottom: 5px double #FFF;
    color: #FFF;
    font-weight: 800;
    &_col {
      margin: 0 20px;
      display: flex;
      flex-basis: 37%;
      align-items: center;
      text-align: left;
      height: 100%;
      font-size: 20px;
      &:first-child {
        flex-basis: 5%;
      }
      &:nth-child(4){
        margin: 0 20px 0 50px;
      }
      &:last-child {
        flex-basis: 7%;
      }
      &-img {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-self: center;
        width: 25px;
        height: 30px;
        margin-left: 10px;
        &--up, &--down {
          display: flex;
          width: 20px;
          height: 20px;
          & img {
            width: 20px;
            height: auto;
          }
        }
        &--up {
          transform: rotate(180deg);
        }
      }
    }
  }
  &__body {
    width: 100%;
    display: flex;
    flex-direction: column;
    &_row {
      display: flex;
      width: 100%;
    }
  }
}

</style>
