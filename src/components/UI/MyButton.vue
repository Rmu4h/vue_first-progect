<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-button'
}
</script>

<style>
.btn {
  align-self: center;
  padding: 10px 40px;
  background-color: blue;
  border: 1px solid teal;
  color: white;
}
</style>