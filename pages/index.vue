<template>
  <div style="padding: 50px;">
    <h3>Blog List</h3>
    <p>This blog posts sourced from <a target="_blank" href="https://hytale-api.com/">https://hytale-api.com/</a> . Very big thanks for provide this api for free, so i can use for this purpose.</p>
    <Divider/>
    <Row :gutter="16" style="margin-top: 20px;">
      <Col :md="8" v-for="post in posts" :key="post.id" style="margin-bottom: 20px;">
        <Card>
          <div style="text-align:center">
            <img :src="post.image" style="width: 100%; height: 100; object-fit: cover;">
            <h4>{{ post.title }}</h4>
          </div>
          <Divider/>
          <div v-html="post.excerpt"></div>
          <NuxtLink :to="post.slug">read more...</NuxtLink>
        </Card>
      </Col>
    </Row>
  </div>
</template>

<script>
  const baseImageUrl = "https://cdn.hytale.com/variants/blog_thumb_"

  export default{
    async asyncData({ app }){
      let renderedPost = []
      const posts = await app.$axios.$get('/api/blog/post/published')
      posts.map((v) => {
          v.id = v._id
          v.title = v.title
          v.author = v.author
          v.slug = v.slug
          v.excerpt = v.bodyExcerpt
          v.image = baseImageUrl + v.coverImage?.s3Key

          return v
      })
      return { posts: posts }
    },
  }
</script>

<style>
  body{
    background-color: #eee;
  }
</style>