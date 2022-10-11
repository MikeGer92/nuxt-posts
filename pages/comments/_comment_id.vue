<template>
  <div class="comment">
    <h1 class="comment__title">Детальная информация о комментарии №{{ commentId }}</h1>
    <CommentTable :columns="columns" :item="commentData" />


  </div>
</template>

<script>
import axios from 'axios'
import CommentTable from '~/components/Comments/CommentTable.vue';
export default {
    data() {
        return {
            commentId: this.$route.params.comment_id,
            commentData: {},
            loading: false,
            columns: ["postId", "commentId", "commentName", "email", "commentBody", "GoTo"]
        };
    },
    beforeMount() {
        this.getComment();
    },
    mounted() {
        // this.getComment()
    },
    methods: {
        async getComment() {
            await axios.get(`https://jsonplaceholder.typicode.com/comments/${this.commentId}`)
                .then(res => {
                this.commentData = res.data;
            })
                .catch(err => {
                if (err.response) {
                    console.log(err.message);
                    alert("Ошибка ответа сервера. Проверьте данные запроса!");
                }
                else if (err.request) {
                    console.log(err);
                    alert("Ошибка запроса. Проверьте данные запроса!");
                }
                else {
                    console.log(err);
                    alert("Что-то пошло не так. Проверьте данные и попробуйте снова!");
                }
            });
        },
    },
    components: { CommentTable }
}
</script>

<style lang="scss">
.comment {
  display: flex;
  flex-direction: column;
  align-items: center;
  &__title {
    margin: 50px 0;
  }
}

</style>
