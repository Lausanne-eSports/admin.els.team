<template>
  <div class="flex flex-col">
    <Heading>Modification of an article</Heading>
    <Spacer direction="b" :size="8" />

    <div class="flex mb-8">
      <nuxt-link
        class="inline-flex shadow items-center justify-center hover:bg-gray-200 transition rounded bg-white px-4 py-2 mr-4"
        to="/articles"
      >
        <Icon name="cheveron-left" class="h-8 mr-1" />
        <span>Back</span>
      </nuxt-link>

      <button
        class="inline-flex items-center shadow bg-blue-800 hover:bg-blue-900 transition rounded text-white px-4 py-2"
        @click="save"
      >Save</button>
    </div>

    <section v-if="!article" class="p-8 mb-8 bg-white shadow rounded w-full">
      <div class="flex items-center justify-center">
        <Loader color="#4299e1" />
      </div>
    </section>

    <Panel v-else header="Metadata">
      <div class="flex justify-between">
        <div class="w-full mr-8">
          <searchable-select
            label="Category"
            name="category_id"
            :items="articleCategories"
            v-model="form.category_id"
          ></searchable-select>
        </div>

        <div class="w-full mr-8">
          <searchable-select
            label="Template"
            name="template_id"
            :items="articleTemplates"
            v-model="form.template_id"
          ></searchable-select>
        </div>

        <div class="w-full">
          <DateTimePicker
            label="Release Date"
            name="published_at"
            v-model="form.published_at"
            :hasTime="true"
          />
        </div>
      </div>

      <div class="flex justify-between">
        <div class="w-full mr-8">
          <base-input label="Thumbnail URL" name="thumbnail" v-model="form.thumbnail"></base-input>
        </div>

        <div class="w-full">
          <base-input
            label="Featured Thumbnail URL"
            name="featured_thumbnail"
            v-model="form.featured_thumbnail"
          ></base-input>
        </div>
      </div>
    </Panel>
  </div>
</template>

<script>
import slug from '@slynova/slug'
import BaseInput from '@/components/Form/BaseInput'
import SearchableSelect from '@/components/Form/SearchableSelect'
import DateTimePicker from '@/components/Form/DateTimePicker'

export default {
  layout: 'app',

  components: {
    BaseInput,
    DateTimePicker,
    SearchableSelect,
  },

  data: () => ({
    form: {
      thumbnail: '',
      featured_thumbnail: '',
      published_at: null,
      template_id: 1,
      category_id: 1,
    },
    article: null,
    articleCategories: [],
    articleTemplates: [],
    errors: null,
  }),

  async created() {
    this.$axios.$get('articles/categories').then(response => {
      this.articleCategories = response
    })

    this.$axios.$get('articles/templates').then(response => {
      this.articleTemplates = response
    })

    this.article = await this.$axios.$get(
      `admin/articles/${this.$route.params.id}`
    )

    this.hydrate()
  },

  methods: {
    hydrate() {
      this.form.thumbnail = this.article.thumbnail
      this.form.featured_thumbnail = this.article.featured_thumbnail
      this.form.published_at = this.article.published_at
      this.form.template_id = this.article.template_id
      this.form.category_id = this.article.category_id
    },

    async save() {
      try {
        await this.$axios.$put(`admin/articles/${this.article.id}`, this.form)
        this.$toast.success('Article saved!')
      } catch (e) {
        console.log(e)
        this.errors = e.response.data.errors
        this.$toast.error('Something went wrong')
      }
    },
  },
}
</script>

