<template>
  <div class="comment-table">
    <div class="comment-table__header">
      <div class="comment-table__header_col" v-for="col in columns" :key="col">{{ capitalize(col) }}
      </div>
    </div>
    <div class="comment-table__body">
      <Comment :comment="item"/>
    </div>

  </div>
</template>

<script>
import Comment from '@/components/Comments/Comment.vue'
export default {
  name: 'CommentTable',
  components: { Comment },
  props: {
    columns: {
      type: Array,
      default: () => []
    },
    item: {
      type: Object,
      default: () => {}
    }

  },
  data() {
    return {

    }
  },
  methods: {
    capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    },
        sendClickUp(col) {
      this.$emit('clickUp', col)
    },
    sendClickDown(col) {
      this.$emit('clickDown', col)
    },
  }

}
</script>

<style lang="scss">
.comment-table {
  display: flex;
  flex-direction: column;
  width: 90%;
  max-width: 1600px;
  border: 2px solid teal;
  &__header {
    display: flex;
    width: 100%;
    height: 70px;
    background: teal;
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
