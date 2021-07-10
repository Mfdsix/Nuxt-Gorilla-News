<template>
  <div style="padding: 50px;">
    <h3>Blog List</h3>
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
          <a target="_blank" :href="post.link">read more...</a>
        </Card>
      </Col>
    </Row>
  </div>
</template>

<script>
  export default{
    async asyncData({ app }){
      let renderedPost = []
      const posts = await app.$axios.$get('https://gorillalogic.com/wp-json/wp/v2/posts')
      posts.forEach(async (v) => {
        const featured = await app.$axios.$get(v._links["wp:featuredmedia"][0].href);
        renderedPost.push({
          title: v.title.rendered,
          excerpt: v.excerpt.rendered,
          link: v.link,
          image: featured?.guid?.rendered,          
        })
      })

      return { posts: renderedPost }
    },
  }
</script>

<style>
  body{
    background-color: #eee;
  }
</style>