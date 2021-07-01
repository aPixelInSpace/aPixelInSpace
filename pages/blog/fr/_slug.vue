<template>
  <v-app>
    <article>
      FR FR FR FR FR FR

      <!-- <AppSearchInput /> -->

      <!-- <v-btn color="primary">Primary</v-btn> -->

      <!-- <h1>
        <v-tooltip bottom>
          <template #activator="{ on, attrs }">
            <span v-bind="attrs" v-on="on">{{ article.title }}</span>
          </template>
          <span>Tooltip</span>
        </v-tooltip>
      </h1> -->

      <h1>{{ article.title }}</h1>

      <p>{{ $t('welcome') }}</p>

      <p>{{ article.description }}</p>
      <img
        class="img-header"
        :src="require(`~/static/blog/${article.img}`)"
        :alt="article.alt"
      />
      <p>
        Image by
        <a target="_blank" :href="article.imgShoutout">{{
          article.imgShoutoutName
        }}</a>
      </p>
      <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

      <nuxt-link :to="switchLocalePath('fr')">Français</nuxt-link>
      <!-- Table des matières
      <nav>
        <ul>
          <li v-for="link of article.toc" :key="link.id">
            <NuxtLink
              :class="{
                'py-2': link.depth === 2,
                'ml-2 pb-2': link.depth === 3,
              }"
              :to="`#${link.id}`"
              >-> {{ link.text }}</NuxtLink
            >
          </li>
        </ul>
      </nav> -->

      <nuxt-content :document="article" />

      <author :author="article.author"></author>

      <prev-next :slug-name="slugName" :prev="prev" :next="next" locale="" />
    </article>
  </v-app>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles/fr', params.slug).fetch()

    const [prev, next] = await $content('articles/fr')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()

    const slugName = 'blog-fr-slug'

    return { article, prev, next, slugName }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('fr', options)
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
