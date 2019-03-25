<template>
  <div :key="post.id">
    <h3>{{ post.title }}</h3>
    <p>{{ post.content }}</p>
    <span>{{ post.timestamp }}</span>
    <div>
      <label>Author</label>
      <p>{{ `${post.user.firstName} ${post.user.lastName}` }}</p>
    </div>
    <div class="row">
      <button class="button-outline">Edit</button>
      <button class="button-clear" @click="deletePost">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PostItem",
  methods: {
    deletePost(){
      this.$apollo.mutate({
        mutation: require('../graphql/deletePost.gql'),
        variables: {
          postId: this.post.id
        },
        refetchQueries: ["getPosts"]
      })
    }
  },
  props: ["post"]
};
</script>
<style>
.margin-right {
  margin-right: 20px;
}
.row {
  justify-content: center;
}

</style>
