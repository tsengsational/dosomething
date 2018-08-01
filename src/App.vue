<template>
  <div id="app">
    <div class="title">
      <div class="title-content">
        {{campaignTitle}}
      </div>
    </div>
    <div class="cards-container" >
      <card v-for="(post, key) in displayPosts" :key="key" :post="post"></card>
    </div>
    <div class="controls-container">
      <button class="previous btn" @click.prevent="handleControlClick" >Previous</button>
      <button class="next btn" @click.prevent="handleControlClick" >Next</button>
    </div>
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
      currentPage: 0,
      campaignTitle: "#MyBigRegret"
    }
  },
  created() {
    console.log('created')
    fetch('http://graphql.dosomething.org/graphql?query={posts(campaignId: "7978") {status createdAt reactions url (w: 300, h: 300) text user { firstName}}}')
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
        for (let i = 0; i < this.acceptedPosts.length; i += 9) {
            let page = this.acceptedPosts.slice(i, i + 9);
            tempArray.push(page);
        }
      }
      return tempArray;
    },
    displayPosts() {
      return this.pages[this.currentPage]
    },
    acceptedPosts() {
      let tempArray = [];
      if (this.posts) {
        tempArray = this.posts.filter(post => post.status === "ACCEPTED")
      }
      return tempArray
    }
  },
  methods: {
    handleControlClick(event) {
      const type = event.target.classList[0]
      switch (type) {
        case "previous":
          if (this.currentPage > 0) {
            this.currentPage--
          }
          break;
        case "next":
          if (this.currentPage < this.pages.length -1) {
            this.currentPage++
          }
        default:
          break;
      }
    }
  }
}
</script>

<style lang="scss">
@import './assets/settings.scss';

body {
  margin: 0;
}
#app {
  font-family: ProximaNovaReg, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: $black;
  margin-bottom: 60px;
}

.title {
  width: 100vw;
  height: 32px;
  background-color: $purple;
  color: $white;
  @include title(24px);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1;
}

.cards-container {
  display: flex;
  flex-wrap:wrap;
  width: 100vw;
  justify-content: space-between;
  position: relative;
  margin-top: 60px;
}

.controls-container {
  position: absolute;
  right: calc(5vw);
  .btn {
    @include button(70px, 40px, 16px);
  }
  .previous {
    margin-right: 16px;
  }
}

@media (min-width: 500px) {
  .cards-container {
    width: 1000px;
    left: calc(50vw - 500px)
  }
  .title {
    height: 40px;
  }
  .controls-container {
    right: calc(50vw - 500px);
  }
}
</style>
