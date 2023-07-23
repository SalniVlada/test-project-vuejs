<script>
import { format } from "date-fns";

export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
  methods: {
    formatDate(strDate) {
      return format(new Date(strDate), "MMMM do, yyyy");
    },
  },
};
</script>

<template>
  <div class="col-md-12 col-sm-12 col-xs-12">
    <div class="single-blog">
      <div class="single-blog-img">
        <RouterLink :to="{ name: 'blog-details', params: { id: post.id } }"
          ><img :src="post.image" alt="" onerror="replaceImageByDefault(this)"
        /></RouterLink>
      </div>
      <div class="blog-meta">
        <span class="comments-type">
          <i class="bi bi-chat"></i>
          <a href="#">11 comments</a>
        </span>
        <span class="date-type">
          <i class="bi bi-calendar"></i>{{ formatDate(post.createdAt) }}
        </span>
      </div>
      <div class="blog-text">
        <h4>
          <a href="#">{{ post.title }}</a>
        </h4>
        <p>
          {{ post.description }}
        </p>
      </div>
      <span>
        <RouterLink
          class="ready-btn"
          :to="{ name: 'blog-details', params: { id: post.id } }"
          >Read more</RouterLink
        >
      </span>
    </div>
  </div>
</template>