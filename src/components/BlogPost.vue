<template>
  <transition name="post">
    <article v-if="post" class="post">
      <header class="post__header">
        <h2 class="post__title">{{ title }}</h2>

        <h3 class="post__meta">
          Date : <time>{{ date }}</time>
          <span class="post__sep"></span>
          Lieu : {{ lieu }}
        </h3>

        <blockquote class="post__subtitle">{{ description }}</blockquote>
      </header>

      <section class="post__body rte" v-html="content"></section>

      <!-- <vue-picture-swipe :items="items"></vue-picture-swipe> -->

<ul>
  <li v-for="image in items">
    <img :src="image.thumbnail" width="300" style="margin:10px; max-width: 300px; float: left;" />
  </li>
</ul>

      <footer class="post__footer">
       <!--  <vue-disqus v-if="commentsReady" shortname="vue-blog-demo"
          :key="post" :identifier="post" :url="`https://vue-blog-demo.netlify.com/read/${post}`"/> -->
      </footer>
    </article>
  </transition>
</template>

<script>
// import VueDisqus from 'vue-disqus/VueDisqus'
// import VuePictureSwipe from 'vue-picture-swipe'
import { kebabify, prettyDate } from '../helpers'

export default {
  name: 'blog-post',
  resource: 'BlogPost',
  // components: { VueDisqus },
  // components: { VuePictureSwipe },
  props: { post: String },

  data() {
    return {
      title: '',
      author: '',
      content: '',
      date: '',
      lieu: 'a',
      published: '',
      description: '',
      commentsReady: false,
      items: ''
    }
  },

  watch: {
    post(to, from) {
      if (to === from || !this.post) return;

      this.commentsReady = false
      this.$getResource('post', to)
        .then(this.showComments)
    }
  },

  methods: {
    kebabify,
    prettyDate,
    showComments() {
      setTimeout(() => {
        this.commentsReady = true
      }, 1000)
    }
  },

  beforeMount() {
    if (!this.post) return;
    this.$getResource('post', this.post)
      .then(this.showComments)
  }
}
</script>
