<template>
  <div id="app">
    <card v-for="(post, key) in displayPosts" :key="key" :post="post"></card>
  </div>
</template>

<script>
import Card from './components/Card'

export default {
  name: 'app',
  components: {
    Card
  },
  data() {
    return {
      posts: null,
      currentPage: 0
    }
  },
  created() {
    console.log('created')
    fetch('http://graphql.dosomething.org/graphql?query={posts(campaignId: "7978") { createdAt reactions url (w: 300, h: 300) text user { firstName}}}')
      .then((resp) => {
        return resp.json()
      })
      .then((json) => {
        this.posts = json.data.posts
      })
      .catch(error => console.log(error))
  },
  computed: {
    pages() {
      let tempArray = [];
      if (this.posts) {
        for (let i = 0; i < this.posts.length; i += 9) {
            console.log(i, this.posts, tempArray)
            let page = this.posts.slice(i, i + 9);
            tempArray.push(page);
        }
      }
      return tempArray;
    },
    displayPosts() {
      return this.pages[this.currentPage]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
