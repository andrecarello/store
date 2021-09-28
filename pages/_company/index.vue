<template>
  <div class="w-90vw max-w-560px mx-auto">
    {{ company }}
    <header class="py-40px">
      <nuxt-link :to="`/${ company.social_url }`">
        <img :src="`https://assets.suitesha.re/${ company.logo }`" alt="" class="h-160px w-160px block mx-auto object-contain">
      </nuxt-link>
      <h1 class="text-dark-500 text-20px text-center font-500">{{ company.name }}</h1>
    </header>

    <section>
      <div v-for="category in categories">
        <nuxt-link :to="`/${ company.social_url }/categories/${ category.id }`" class="relative">
          <img v-if="!category.image_url" src="https://store.suitesha.re/img/not-found.b5c89d1c.jpg" alt="" class="h-150px w-full shadow-xl mb-4 object-cover" />
          <img v-else :src="`https://assets.suitesha.re/${ category.image_url }`" alt="" class="h-150px w-full shadow-xl mb-4 object-cover" />
          <p>{{ company.description }}</p>
        </nuxt-link>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  name: "AliasView",
  layout: 'default',

  // TODO - Validar strict das informações vindas das api`s
  async asyncData({ $axios, params }) {
    let company = {};
    let stores = {};
    let categories = []

    try {
      company = await $axios.get(`/companies/${ params.company }`)
    } catch (error) {
      console.log(error)
    }

    try {
      stores = await $axios.get(`/company/${ company.data.id }/stores`)
    } catch (error) {
      console.log(error)
    }

    try {
      categories = await $axios.get(`/store/${ stores.data[0].id }/categories`)
    } catch (error) {
      console.log(error)
    }

    return {
      company: company.data,
      stores: stores.data,
      categories: categories.data
    }
  },

  head() {
    return {
      title: `${ this.company.name } - Store`,
      meta: [
        {
          name: 'description',
          content: `${ this.company.name }`
        }
      ]
    }
  },
}
</script>
