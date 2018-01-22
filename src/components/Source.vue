<template>
    <section class="hero is-dark">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            <a :href="source.url">{{source.name}}</a>
            <span class="tag is-light is-rounded">{{source.category}}</span>
          </h1> 
          <p class="subtitle">
            {{source.description}}
          </p>

        </div>
      </div>
    </section>
</template>

<script>
export default {
  props: {
    source: { required: true }
  },
  name: 'Source'

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
