<template>
  <div class="index">
    <article-list :article="article" @loadmore="loadmoreArticle"></article-list>
  </div>
</template>

<script>
  import Carrousel from '~components/article/archive/carrousel'
  import ArticleList from '~components/article/archive/list'

  export default {
    name: 'data-article-list',
    validate ({ params }) {
      return !Object.is(new Date(params.date).toString(), 'Invalid Date')
    },
    fetch({ store, params }) {
      return store.dispatch('loadArticles', params)
    },
    head() {
      return { title: `${this.defaultParams.date} | Date` }
    },
    components: {
      Carrousel,
      ArticleList
    },
    computed: {
      article() {
        return this.$store.state.article.list
      },
      defaultParams() {
        return {
          date: this.$route.params.date
        }
      },
      nextPageParams() {
        return Object.assign({
          page: this.article.data.pagination.current_page + 1
        }, this.defaultParams)
      }
    },
    methods: {
      loadmoreArticle() {
        this.$store.dispatch('loadArticles', this.nextPageParams)
      }
    }
  }
</script>
