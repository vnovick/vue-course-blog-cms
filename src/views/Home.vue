<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <AddPost />
    <PostsList />
  </div>
</template>

<script>
// @ is an alias to /src
import PostsList from '@/components/PostsList.vue'
import AddPost from '@/components/AddPost.vue'

export default {
  name: 'home',
  components: {
    PostsList,
    AddPost
  }
}
</script>
