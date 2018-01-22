<template>
  <div class="container is-fluid">
    <section class="hero is-primary is-medium">
      <div class="hero-body hero-spec">
        <div class="container has-text-centered">
          <h1 class="title">
             <i class="fa fa-newspaper-o"></i> News
          </h1>
          <h2 class="subtitle">
            
          </h2>

          <div class="control">
            <label class="radio">
              <input type="radio" name="categ" value="" v-model="selectedCategory">
              all
            </label>
            <label class="radio" v-for="category in categories">
              <input type="radio" name="categ" :value="category.value" v-model="selectedCategory">
              {{ category.name }}
            </label>
          </div>

          <div class="control">
            <label class="radio" v-for="language in languages.slice(0, 3)">
              <input type="radio" name="language" :value="language.value" v-model="selectedLanguage">
              {{ language.text }}
            </label>
          </div>

          <div class="field">
            <p class="control">
              <span class="select" :class="{ 'is-danger': selectedSource === false}">
                <select v-model="selectedSource">
                  <option value="" selected disabled hidden>Select source</option>
                  <option v-for="source in sourceList" :value="source.id">
                    {{ source.name }}
                  </option>
                </select>
              </span>
            </p>
          </div>

          <p class="help is-dark" v-if="selectedSource === false">This input is invalid</p>

        </div>
      </div>
    </section>

    <source-info :source="wholeSource" v-if="wholeSource !== undefined"></source-info>

    <main class="section">

      <div class="container">
        <div class="columns is-centered has-text-centered" v-if="wholeSource !== undefined">
          <div v-if="wholeSource.sortBysAvailable.length > 1">
            <p>Order by: </p>
            <div class="control">
              <label class="radio" v-for="ordering in wholeSource.sortBysAvailable">
                <input type="radio" name="orderBy" :value="ordering" v-model="selectedOrder">
                {{ ordering }}
              </label>   
              <!-- TODO !!!!! -->
            </div>
          </div>
        </div>

        <div class="columns is-centered"  v-for="single in articleList">
          <div class="column is-half">
            <custom-article :singleNews="single"></custom-article>
          </div>
        </div>


      </div>
    </main>
    <footer class="footer">
      <div class="container">
        <div class="content has-text-centered">
          <p>
            Made by: <strong>Bojan Jakic</strong> using<br>
            <strong>Bulma</strong> by <a href="http://jgthms.com" target="_blank">Jeremy Thomas</a>. <br>
            Powered by: <strong><a href="https://newsapi.org/" target="_blank">News API</a></strong>
          </p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'
import Article from './components/Article'
import Source from './components/Source'

export default {
  name: 'app',
  components: {
    'custom-article': Article,
    'source-info': Source
  },
  mounted () {
    this.getSourceList()
  },
  watch: {
    selectedCategory (to, from) {
      this.getSourceList()
    },
    selectedLanguage (to, from) {
      this.getSourceList()
    },
    selectedSource (to, from) {
      this.getSourceObject()
      this.getArticleList()
      this.selectedOrder = this.wholeSource.sortBysAvailable[0]
    },
    selectedOrder (to, from) {
      this.getArticleList()
    }
  },
  data () {
    return {
      categories: [
        { value: 'business', name: 'business' },
        { value: 'entertainment', name: 'entertainment' },
        { value: 'gaming', name: 'gaming' },
        { value: 'general', name: 'general' },
        { value: 'music', name: 'music' },
        { value: 'politics', name: 'politics' },
        { value: 'science-and-nature', name: 'science and nature' },
        { value: 'sport', name: 'sport' },
        { value: 'technology', name: 'technology' }
      ],
      languages: [
        { value: '', text: 'All' },
        { value: 'en', text: 'English' },
        { value: 'de', text: 'German' },
        { value: 'fr', text: 'French' }
      ],
      sourceList: [],
      articleList: [],
      selectedCategory: '',
      selectedLanguage: '',
      selectedOrder: '',
      selectedSource: '',
      wholeSource: undefined
    }
  },
  computed: {
    categoryParam () {
      return 'category=' + this.selectedCategory
    }
  },
  methods: {
    getSourceList () {
      axios({
        method: 'get',
        url: 'https://newsapi.org/v1/sources',
        params: {
          category: this.selectedCategory,
          language: this.selectedLanguage
        },
        responseType: 'stream'
      })
        .then(response => {
          console.log(response)
          this.sourceList = response.data.sources
        })
    },
    getArticleList () {
      if (this.selectedSource === '') {
        this.selectedSource = false
        return
      }
      axios({
        method: 'get',
        url: 'https://newsapi.org/v1/articles',
        params: {
          source: this.selectedSource,
          apiKey: '0db4997140bb41e4ae5fd67f0877533a',
          sortBy: this.selectedOrder
        },
        responseType: 'stream'
      })
        .then(response => {
          console.log(response)
          this.articleList = response.data.articles
        })
    },
    getSourceObject (e) {
      for (let i = 0; i < this.sourceList.length; i++) {
        if (this.sourceList[i].id === this.selectedSource) {
          this.wholeSource = this.sourceList[i]
        }
      }
    }
  }
}
</script>

<style>
.hero.is-primary a:not(.button), 
.hero.is-primary strong{
  color: black;
}

.control{
  text-align: center;
  margin-bottom: 20px;
}

.hero-spec{
  padding-bottom: 2rem !important;
}

label{
  text-transform: capitalize;
}

.footer{
  padding: 2rem 1.5rem;
}

.field{
  width: 200px;
  margin: auto;
}

.title{
  margin-bottom: 3rem !important;
}
</style>
