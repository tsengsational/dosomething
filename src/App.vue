<template>
  <div id="app">
    <div class="title">
      <img :src="logo" class="logo">
    </div>
    <div class="hero" >
      <div class="hero-content">
        <div class="campaign-title">
          {{campaignTitle}}
        </div>
        <div class="campaign-tagline" >The FDA missed a chance to ban menthol in 2011. Here's what you had to say about it</div>
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
import Logo from './assets/logo.js'

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
    },
    logo() {
      return Logo.image
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
  margin-bottom: 80px;
}

.title {
  width: 100vw;
  height: 40px;
  background-color: $white;
  color: $white;
  @include title(24px);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10;
  .logo {
    position: relative;
    top: 5px;
    left: 5vw;
    max-height: 25px 
  }
}

.hero {
  height: 300px;
  width: 100vw;
  margin-top: 40px;
  background-image: url("https://images.ctfassets.net/81iqaqpfd8fy/2VUWEkSWAgW4qIQCUyUIgA/90643e3aa19bb6eaeab961f31b6ad2da/_MyBigRegret_Header_1.jpg?w=1440&h=810&fit=fill");
    background-size: cover;
    background-position: center;
    position: relative;
    box-sizing: border-box;
    &::after {
      content: "";
      height: 300px;
      width: 100vw;
      position: absolute;
      z-index: 1;
      top: 0px;
      background: linear-gradient($transparent, $black);
      opacity: .5;
    }
  .hero-content {
      position: absolute;
      z-index: 2;
      top: 50%;
      width: 75%;
      left: calc(25%/2);
      color: $white;
      .campaign-title {
        @include title(32px);
      }
      .campaign-tagline {
        @include body(16px);
      }
  }
}

.cards-container {
  display: flex;
  flex-wrap:wrap;
  width: 100vw;
  justify-content: space-around;
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
    left: calc(50vw - 500px);
    justify-content: space-between;
    align-content: space-between;
    display: grid;
    grid-template-columns: 300px 300px 300px;
    // grid-template-rows: 1fr 1fr 1fr;
  }
  .title {
    height: 70px;
    .logo {
      max-height: 60px;
    }
  }

  .hero {
    height: 620px;
    margin-top: 70px;
    &::after {
      height: 620px;
    }
    .hero-content {
      position: absolute;
      z-index: 2;
      bottom: 24px;
      margin-left: 12.5vw;
      font-size: 64px;
      width: 50%;
      .campaign-title {
        font-size: 54px;
      }
      .campaign-tagline {
        font-size: 24px;
        font-weight: 600;
      }
    }
  }
  .controls-container {
    right: calc(50vw - 500px);
  }
}
</style>
