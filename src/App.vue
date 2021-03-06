<template>
  <div id="app">
    <div class="title">
      <a href="https://www.dosomething.org"></a>
      <img :src="logo" class="logo">
      <ul>
        <li>
          <a href="https://www.dosomething.org/us/campaigns">
            <div>Explore Campaigns</div>
            <span>Find ways to take action both online and off.</span>
          </a>
        </li>
        <li>
          <a href="https://www.dosomething.org/us/about/who-we-are">
            <div>What is DoSomething.org?</div>
            <span>A global movement for good.</span>
          </a>
        </li>
      </ul>
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
    fetch('https://graphql.dosomething.org/graphql?query={posts(campaignId: "7978") {status createdAt reactions url (w: 300, h: 300) text user { firstName}}}')
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
  @include title(24px);
  background-color: $white;
  color: $white;
  height: 40px;
  left: 0;
  position: fixed;
  width: 100vw;
  top: 0;
  z-index: 10;
  .logo {
    left: 5vw;
    max-height: 25px; 
    position: relative;
    top: 5px;
  }
  ul {
    display: none;
  }
}

.hero {
  background-image: url("https://images.ctfassets.net/81iqaqpfd8fy/2VUWEkSWAgW4qIQCUyUIgA/90643e3aa19bb6eaeab961f31b6ad2da/_MyBigRegret_Header_1.jpg?w=1440&h=810&fit=fill");
  background-position: center;
  background-size: cover;
  box-sizing: border-box;
  height: 300px;
  margin-top: 40px;
  width: 100vw;
  position: relative;
    &::after {
      background: linear-gradient($transparent, $black);
      content: "";
      height: 300px;
      opacity: .5;
      position: absolute;
      top: 0px;
      width: 100vw;
      z-index: 1;
    }
  .hero-content {
      color: $white;
      left: calc(25%/2);
      position: absolute;
      top: 50%;
      width: 75%;
      z-index: 2;
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
  justify-content: space-around;
  margin-top: 60px;
  position: relative;
  width: 100vw;
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
    align-content: space-between;
    display: grid;
    grid-template-columns: 300px 300px 300px;
    justify-content: space-between;
    left: calc(50vw - 500px);
    width: 1000px;
  }
  .title {
    height: 70px;
    .logo {
      max-height: 60px;
    }
    ul {
      display: flex;
      left: 15vw;
      list-style: none;
      list-style-type: none;
      position: absolute;
      top: -10px;
      a {
        text-decoration: none;
      }
      li {
        margin-right: 32px;
        div {
          @include source(18px, 400);
          color: $black;
        }
        span {
          @include source(14px, 200);
          color: $gray;
          position: relative;
          top: -10px;
        }
        
      }
    } 
  }

  .hero {
    height: 620px;
    margin-top: 70px;
    &::after {
      height: 620px;
    }
    .hero-content {
      bottom: 24px;
      font-size: 64px;
      margin-left: 12.5vw;
      position: absolute;
      width: 50%;
      z-index: 2;
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
