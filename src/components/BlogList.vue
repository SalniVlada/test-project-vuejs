<script>
import BlogCard from "./BlogCard.vue";

export default {
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
  components: {
    BlogCard,
  },
};
</script>

<template>
  <div class="col-md-8 col-sm-8 col-xs-12">
    <div class="row">
      <BlogCard v-for="(post, index) in posts" :key="index" :post="post" />
    </div>
  </div>
</template>
