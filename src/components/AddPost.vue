<template>
  <form @submit.prevent="submit">
    <fieldset>
      <input type="text" placeholder="Subject" v-model="subject" />
      <ApolloQuery :query="require('../graphql/getUsers.gql')">
        <template slot-scope="{ result: { data, error, loading }}">
          <div v-if="loading">Loading users</div>
          <select v-else-if="data" v-model="currentUser">
            <option disabled value="">Select user</option>
            <option v-for="user in data.users" :key="user.id" :value="user.id">{{
              `${user.firstName} ${user.lastName}`
            }}</option>
          </select>
        </template>
      </ApolloQuery>
      <textarea placeholder="Content" v-model="content" />
    </fieldset>
    <input
      className="button-primary"
      type="submit"
      value="Send"
      :disabled="currentUser === ''"
    />
  </form>
</template>

<script>

export default {
  name: "AddPost",
  data(){
    return {
      subject: "",
      content: "",
      currentUser: ""
    }
  },
  methods: {
    submit(e) {
      const { subject, content, currentUser: userId} = this
      this.$apollo.mutate({
        mutation: require('../graphql/addPost.gql'),
        variables: {
          subject,
          content,
          userId
        },
      }).then(data => {
        this.currentUser = ''
        this.subject = ''
        this.content = ''
      }).catch(error => {
        console.log(error)
      })
    }
  }
};

</script>