<template>
  <div class="w-90vw max-w-560px mx-auto">
    <header class="py-4">
      <nuxt-link :to="`/${ company.social_url }`">Voltar</nuxt-link>
    </header>

    <div>
      <div class="grid grid-cols-2 gap-4">
        <article v-for="product in category">
          <img :src="`https://assets.suitesha.re/${ product.images[0].image_url }`" />
          <p>{{ product.description }}</p>
          <p>{{ product.value }}</p>
        </article>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CategoryView",

  async asyncData({ $axios, params }) {
    const { id } = params;
    let company = {};
    let category = [];

    try {
      company = await $axios.$get(`/companies/${ params.company }`)
    } catch (error) {
      console.log(error)
    }

    try {
      category = await $axios.$get(`/company/${ company.id }/category/${ id }/products`)
    } catch (error) {
      console.log(error)
    }

    return {
      company,
      category
    }
  },

  head() {
    return {
      title: `${ this.category[0].name } | ${ this.company.name } - Store`,
      meta: [
        {
          name: 'description',
          content: `${ this.category[0].description }`
        }
      ]
    }
  },
}
</script>
