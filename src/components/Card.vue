<template>
    <div class="card" @mouseenter="handleMouseOver" @mouseleave="handleMouseLeave">
        <div class="image" :style="imageStyle" ></div>
        <div class="content" :class="{hover: mouseOver}" >
            <div class="text" >{{post.text}}</div>
            <div class="info" >by <span class="first-name">{{post.user.firstName}}</span> at <span>{{createdAt}}</span> </div>
            <div class="likes" >{{post.reactions}} Likes </div>
        </div>
    </div>    
</template>

<script>
export default {
    props: ["post"],
    data() {
        return {
            mouseOver: false
        }
    },
    computed: {
        createdAt() {
            const date = new Date(this.post.createdAt)
            return date.toLocaleDateString()
        },
        imageStyle() {
            return {
                backgroundImage: `url(${this.post.url})`
            }
        }
    },
    methods: {
        handleMouseOver() {
            this.mouseOver = true;
        },
        handleMouseLeave() {
            this.mouseOver = false;
        }
    }    
}
</script>

<style lang="scss" scoped>
@import '../assets/settings.scss';

.card {
    width: 300px;
    height: 400px;
    margin-bottom: 16px;
    border: 1px $gray solid;
    .image {
        width: 300px;
        height: 300px;
        background-position: center;
        background-size: contain;
    }
    .content {
        box-sizing: border-box;
        padding: 16px;
        background-color: $dark-gray;
        color: $white;
        height: 100px;
        transition: background-color .3s;
        &.hover {
            background-color: $purple;
        }
        .text {
            @include title(16px);
        }
        .info span {
            @include title(16px);
            transition: color .3s;
            &.first-name:hover {
                color: $yellow;
                cursor: pointer;
            }
        }
    }
}

</style>
