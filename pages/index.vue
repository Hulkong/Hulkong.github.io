<template>
  <div class="">
    <!-- <TheHeader /> -->
    <Nav></Nav>
    <ul class="main-layout flex flex-wrap">
      <li
        v-for="article of articles"
        :key="article.slug"
        class="xs:w-full md:w-1/2 px-2 xs:mb-6 md:mb-12 article-card"
      >
        <NuxtLink
          :to="{ name: 'blog-slug', params: { slug: article.slug } }"
          class="flex transition-shadow duration-150 ease-in-out shadow-sm hover:shadow-md xxlmax:flex-col"
        >
          <!-- <img
            v-if="article.renderimg"
            class="h-48 xxlmin:w-1/2 xxlmax:w-full object-contain"
            :src="article.renderimg"
          />
          <img
            v-else
            class="h-48 xxlmin:w-1/2 xxlmax:w-full object-contain"
            :src="article.img"
          /> -->

          <div
            class="list p-6 flex flex-col justify-between xxlmin:w-1/2 xxlmax:w-full"
          >
            <h2 class="title font-bold">{{ article.title }}</h2>
            <!-- <div v-show="isNow(article.date)" class="new">New!</div> -->
            <div class="new">New!</div>
            <p class="font-bold text-gray-600 text-sm">
              {{ article.description }}
            </p>
          </div>
        </NuxtLink>
      </li>
    </ul>
    <h3 class="mb-4 font-bold text-2xl uppercase text-center">Topics</h3>
    <ul class="flex flex-wrap mb-4 text-center">
      <li
        v-for="tag of tags"
        :key="tag.slug"
        class="xs:w-full md:w-1/3 lg:flex-1 px-2 text-center"
      >
        <NuxtLink :to="`/blog/tag/${tag.slug}`" class="">
          <p
            class="font-bold text-gray-600 uppercase tracking-wider font-medium text-ss"
          >
            {{ tag.name }}
          </p>
        </NuxtLink>
      </li>
    </ul>
    <footer class="flex justify-center border-gray-500 border-t-2">
      <p class="mt-4">
        Created by
        <a href="https://github.com/hulkong" class="font-bold hover:underline"
          >Hulkong</a
        >
      </p>
    </footer>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .only([
        'date',
        'title',
        'description',
        'img',
        'slug',
        'author',
        'renderimg'
      ])
      .sortBy('createdAt', 'desc')
      .fetch()
    const tags = await $content('tags', params.slug)
      .only(['name', 'description', 'img', 'slug'])
      .sortBy('createdAt', 'asc')
      .fetch()
    return {
      articles,
      tags
    }
  },

  methods: {
    /**
     * @description Checking Current date
     * @param {Date} date
     */
    isNow(date = null) {
      if (!date) return

      return new Date(date).toDateString() === new Date().toDateString()
    }
  }
}
</script>

<style class="postcss">
.object-contain {
  object-fit: contain;
}

.article-card {
  width: 100%;
  border-radius: 10px;
  cursor: pointer;
  padding: 20px;
  margin: 0;
}

.article-card a {
  background-color: #fff;
  border-radius: 8px;
}
.article-card img div {
  border-radius: 8px 0 0 8px;
}

.main-layout {
  display: flex;
  flex-flow: column;
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
}

.list {
  position: relative;
}

.new {
  position: absolute;
  right: 0;
  color: #26af8a;
  margin: 0 4px 0 0;
  padding: 5px 10px;
  border-radius: 5px;
  max-height: 28px;
}

.title {
  font-size: 1.5rem;
  letter-spacing: -1px;
  color: var(--color-title);
  width: 100%;
  margin: 0;
  max-width: 600px;
}
</style>
