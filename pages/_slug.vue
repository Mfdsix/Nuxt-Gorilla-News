<template>
    <div style="padding: 50px;">
        <Card style="padding: 50px;">
            <NuxtLink to="/"><Icon type="md-arrow-round-back" /> Back to home</NuxtLink>
            <Divider/>
            <div>
                <img :src="post.image" alt="post.title" style="width: 100%">
                <h2 style="margin-top: 20px">{{ post.title }}</h2>
                <p>author: {{ post.author}}</p>
            </div>
            <Divider/>
            <div v-html="post.content"></div>
        </Card>
    </div>
</template>

<script>
    const baseImageUrl = "https://cdn.hytale.com/variants/blog_thumb_"

    export default {
        async asyncData({ params, app }){
            const post = await app.$axios.$get("/api/blog/post/slug/" + params.slug)
            return { post : {
                title: post.title,
                author: post.author,
                image: baseImageUrl + post.coverImage?.s3Key,
                content: post.body,
            }}
        }
    }
</script>