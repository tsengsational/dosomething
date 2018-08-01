<template>
    <div class="card">
        <div class="image" :style="imageStyle" ></div>
        <div class="content" >
            <div class="text" >{{post.text}}</div>
            <div class="info" >by {{post.user.firstName}} at {{post.createdAt}} </div>
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
    }    
}
</script>

<style lang="scss" scoped>
    .card {
        width: 300px;
        height: 400px;
        .image {
            width: 300px;
            height: 300px;
            background-position: center;
            background-size: contain;
        }
        .content {
            box-sizing: border-box;
            padding: 16px 0;
        }
    }

</style>
