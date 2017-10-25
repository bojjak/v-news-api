<template>

      <div class="box">
        <article class="media">
          <div class="media-left">
            <figure class="image is-128x128">
              <a :href="singleNews.url" target="_blank">
                <img :src="singleNews.urlToImage" alt="Image" class="img">
              </a>
            </figure>
          </div>
          <div class="media-content">
            <div class="content">
              <p>
                <small>{{getCustomDate}}</small> <br> 
                <a :href="singleNews.url" target="_blank">
                  <strong>{{ singleNews.title }}</strong>
                </a> <br>
                <small v-if="singleNews.author">@{{singleNews.author}}</small>  
                <br>
              </p>
              <p>{{ singleNews.description }}</p>
              <a :href="singleNews.url" target="_blank">Read more...</a>
            </div>
<!--             <nav class="level is-mobile">
              <div class="level-left">
                <a class="level-item">
                  <span class="icon is-small"><i class="fa fa-reply"></i></span>
                </a>
                <a class="level-item">
                  <span class="icon is-small"><i class="fa fa-retweet"></i></span>
                </a>
                <a class="level-item">
                  <span class="icon is-small"><i class="fa fa-heart"></i></span>
                </a>
              </div>
            </nav> -->
          </div>
        </article>
      </div>
</template>

<script>
export default {
  props: {
    singleNews: { required: true }
  },
  name: 'Article',
  computed: {
    getCustomDate () {
      let date = new Date(this.singleNews.publishedAt)
      return date.getDate() + '. ' + date.getMonth() + '. ' + date.getFullYear() + '.'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.box{
  transition: .2s all ease-in-out;  
}
.box:hover {
    box-shadow: 0 3px 15px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
}
</style>
