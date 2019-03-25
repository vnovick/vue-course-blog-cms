<template>
  <div>
    <ApolloQuery :query="require('../graphql/getPosts.gql')">
      <ApolloSubscribeToMore :document="require('../graphql/getLastPost.gql')" :updateQuery="onPostAdded"/>
      <template slot-scope="{result: { data, error, loading }}">
        <div v-if="loading">Loading...</div>
        <div v-else-if="error">Error...</div>
        <div v-else-if="data">
          <ul class="column">
            <li v-for="post in data.posts" :key="post.id">
              <PostItem :post="post"/>
            </li>
          </ul>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import PostItem from './PostItem'
import gql from 'graphql-tag'
export default {
  name: 'HelloWorld',
  data(){
    return {
      posts: []
    }
  },
  methods: {
    onPostAdded(previousResult, { subscriptionData }){
      if(previousResult && previousResult.posts[0].id !== subscriptionData.data.posts[0].id){
        return {
          posts: [
            ...subscriptionData.data.posts,
            ...previousResult.posts
          ]
        }
      }
    }
  },
  components: {
    PostItem
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.column {
  display: flex;
  flex-direction: column;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
