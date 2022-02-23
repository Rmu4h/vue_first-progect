<template>
    <div v-show="posts.length > 0">
      <h3>List of users</h3>
      <transition-group name="list-complete">
        <post-item 
          v-for="post in posts" 
          :post="post"
          :key="post.id"
          @remove="$emit('remove', post)"
        />
      </transition-group>
    </div>
    <h2 v-show="posts.length === 0" style="color: red">
      List is empty
    </h2>
</template>

<script>
import PostItem from './PostItem.vue'
export default {
  components: { PostItem },
  props: {
    posts: {
      type: Array,
      required: true,
    }
  }
}
</script>

<style scoped>


.list-complete-enter-from,
.list-complete-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.list-complete-leave-active {
  position: absolute;
}
</style>