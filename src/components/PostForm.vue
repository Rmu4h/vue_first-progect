<template>
  <form class="form" @submit.prevent>
    <h2>Create post</h2>
    <my-input
      v-model="post.title"
      type="text" 
      placeholder="Name of post"
    />
    <my-input 
      v-model="post.body"
      type="text" 
      placeholder="Description of post"
    />
    <my-button 
      class="btn"
      @click="createPost"
    >
    Add
    </my-button>
  </form>
</template>

<script>
import MyButton from './UI/MyButton.vue';
import MyInput from './UI/MyInput.vue';
export default {
  components: { MyButton, MyInput },
  data() {
    return {
      post: {
        title: '',
        body: '',
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: '',
      }
    }
  },
}
</script>

<style>
.form {
  display: flex;
  flex-direction: column;
}

.btn {
  margin: 20px 0;
}



</style>