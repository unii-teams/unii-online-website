<template>
  <div class="col mb-4">
    <div class="card h-100 shadow-sm">
      <img
        :src="`https://api.unii.co.th/api/uploads/${post.image}`"
        class="card-img-top"
        alt=""
        height="200"
        style="object-fit: cover; object-position: 0px 0%"
      />
      <div class="card-body">
        <h5 class="card-title">{{ post.title }}</h5>
        <small class="text-muted">{{ post.createdAt }}</small>
        <!-- eslint-disable-next-line vue/no-v-html -->
        <p class="card-text" v-html="post.description">
          <!-- {{ post.description }} -->
        </p>
        <NuxtLink
          :to="{ name: 'blogs-id', params: { id: post._id } }"
          target="_blank"
          class="stretched-link"
        ></NuxtLink>
        <!-- <a href="#" class="stretched-link"></a> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // eslint-disable-next-line vue/require-default-prop
    post: Object,
  },
}
</script>

<style scoped>
.card {
  border-radius: 5px;
  border: none;
}
.card:hover {
  transform: scale(1.025);
}
.card-title {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
.card-text {
  display: -webkit-box;
  -webkit-line-clamp: 5;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
