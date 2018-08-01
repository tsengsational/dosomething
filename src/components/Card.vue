<template>
    <div class="card">
        <div class="image" :style="imageStyle" ></div>
        <div class="content" >
            <div class="text" >{{post.text}}</div>
            <div class="info" >by <span>{{post.user.firstName}}</span> at <span>{{createdAt}}</span> </div>
            <div class="likes" >{{post.reactions}} Likes </div>
        </div>
    </div>    
</template>

<script>
export default {
    props: ["post"],
    data() {
        return {
            imageStyle: {
                backgroundImage: `url(${this.post.url})`
            }
        }
    },
    computed: {
        createdAt() {
            const date = new Date(this.post.createdAt)
            return date.toLocaleDateString()
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
        .text {
            @include title(16px);
        }
        .info span {
            @include title(16px);
        }
    }
}

</style>
