<template>
  <v-app>
    <article>
      <AppSearchInput />

      <v-btn color="primary">Primary</v-btn>

      <h1>
        <v-tooltip bottom>
          <template #activator="{ on, attrs }">
            <span v-bind="attrs" v-on="on">{{ article.title }}</span>
          </template>
          <span>Tooltip</span>
        </v-tooltip>
      </h1>

      <p>{{ article.description }}</p>
      <img class="img-header" :src="article.img" :alt="article.alt" />
      <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

      Table des matières
      <nav>
        <ul>
          <li v-for="link of article.toc" :key="link.id">
            <NuxtLink
              :class="{ 'py-2': link.depth === 2, 'ml-2 pb-2': link.depth === 3 }"
              :to="`#${link.id}`"
              >-> {{ link.text }}</NuxtLink
            >
          </li>
        </ul>
      </nav>

      <!-- <br v-for="n in 40" :key="n" /> -->

      <nuxt-content :document="article" />

      <!-- <br v-for="n in 40" :key="n" /> -->

      <author :author="article.author"></author>

      <prev-next :prev="prev" :next="next" />
    </article>
  </v-app>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()

    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()

    return { article, prev, next }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    },
  },
}
</script>

<style>
.nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
}
.nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}
.nuxt-content p {
  margin-bottom: 20px;
}
.icon.icon-link {
  background-image: url('~assets/svg/icon-hashtag.svg');
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
.img-header {
  width: 300px;
}
</style>
