<template>
  <section id="posts">
    <PostPreview
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :excerpt="post.previewText"
      :thumbnailImage="post.thumbnailUrl"
      :id="post.id" />
  </section>
</template>

<script>
import PostPreview from '@/components/Blog/PostPreview';

export default {
  components: {
    PostPreview
  },
  asyncData(context) {
    return context.app.$storyapi
      .get('cdn/stories', {
        version: 'draft',
        starts_with: 'blog/'
      })
      .then(res => {
        return {
          posts: res.data.stories.map(blogPost => {
            return {
              id: blogPost.slug,
              title: blogPost.content.title,
              previewText: blogPost.content.summary,
              thumbnailUrl: blogPost.content.thumbnail
            };
          })
        };
      });
  }
  // data() {
  //   return {
  //     posts: [
  //       {
  //         title: 'A new Beginning',
  //         previewText: "This will be awesome, dont't miss it",
  //         thumbnailUrl: 'http://www.wolfexperience.com/wp-content/uploads/2016/01/social-media.jpg',
  //         id: 'a-new-beginning'
  //       },
  //       {
  //         title: 'A Second Beginning',
  //         previewText: "This will be awesome, dont't miss it",
  //         thumbnailUrl: 'http://www.wolfexperience.com/wp-content/uploads/2016/01/social-media.jpg',
  //         id: 'a-second-beginning'
  //       }
  //     ]
  //   };
  // }
};
</script>

<style scoped>
#posts {
  padding-top: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

@media (min-width: 35rem) {
  #posts {
    flex-direction: row;
  }
}
</style>
