<template>
  <div class="flex flex-col">
    <Heading>Add a team</Heading>
    <Spacer direction="b" :size="8" />

    <div class="flex mb-8">
      <nuxt-link
        class="inline-flex shadow items-center justify-center hover:bg-gray-200 transition rounded bg-white px-4 py-2 mr-4"
        to="/teams"
      >
        <Icon name="cheveron-left" class="h-8 mr-1" />
        <span>Back</span>
      </nuxt-link>

      <button
        class="inline-flex items-center shadow bg-blue-800 hover:bg-blue-900 transition rounded text-white px-4 py-2"
        @click="save"
      >Save</button>
    </div>

    <Panel header="Information">
      <div class="flex justify-between">
        <div class="w-full mr-8">
          <base-input label="Name" name="name" v-model="form.name"></base-input>
        </div>

        <div class="w-full mr-8">
          <searchable-select
            label="Type"
            name="category_id"
            :items="teamCategories"
            v-model="form.category_id"
          ></searchable-select>
        </div>
      </div>

      <div class="flex flex-col">
        <NewCheckbox label="Academy" name="academy" v-model="form.academy" />
      </div>
    </Panel>
  </div>
</template>

<script>
import NewCheckbox from '@/components/Form/NewCheckbox'
import BaseInput from '@/components/Form/BaseInput'
import SearchableSelect from '@/components/Form/SearchableSelect'

export default {
  layout: 'app',

  components: { BaseInput, NewCheckbox, SearchableSelect },

  data: () => ({
    form: {
      name: null,
      academy: false,
      category_id: null,
    },
    teamCategories: [],
  }),

  async created() {
    this.teamCategories = await this.$axios.$get('teams/categories')
  },

  methods: {
    async save() {
      try {
        const team = await this.$axios.$post('admin/teams', this.form)
        this.$router.push(`/teams/${team.id}/edit`)
        this.$toast.success('Team created!')
      } catch (e) {
        this.$toast.error('Something went wrong')
      }
    },
  },
}
</script>
