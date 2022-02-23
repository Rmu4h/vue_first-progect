<template>
  <div class="item__list  list-complete-item">
    <div class="post">
      <div>{{ post.id }}</div>
      <div><strong>Name</strong> {{ post.title }}</div>
      <div><strong>Description</strong> {{ post.body }}</div>
    </div>

    <div class="post__buttons">
      <my-button
        @click="$emit('remove', post)"
      >
        delete
      </my-button>
    </div>
  </div>
</template>

<script>
import MyButton from './UI/MyButton.vue'
export default {
  components: { MyButton },

  props: {
    post: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style scoped>
  .item__list {
    display: flex;

    justify-content: space-between;
    background-color: lightblue;
    border: 2px solid blue;
    /* padding: 10px; */
    width: 50%;
    margin: 0 auto;
    margin-top: 15px;
    align-items: center;
    padding: 10px 0;
  }
  .post {
    display: flex;
    flex-direction: column;
    text-align: left;
    margin-left: 30px;
  }

  .post__buttons {
    margin-right: 30px;
  }

  .list-complete-item {
    transition: all 0.8s ease;
    display: inline-block;
    margin-right: 10px;
  }
</style>