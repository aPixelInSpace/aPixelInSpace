<template>
  <div>
    <NuxtLink :to="`/blog/author/${author.name}`">
      <img class="imgHead" :src="author.img" />
      <div>
        <h4>Author</h4>
        <p>{{ author.name }}</p>
        <p>{{ author.bio }}</p>
      </div>
    </NuxtLink>
  </div>
</template>

<script>
export default {
  props: {
    author: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style scoped>
.imgHead {
  width: 3rem;
}
</style>
