<template>
  <tr class="h-16 border-b border-neutral-200">
    <td>{{ stream.username }}</td>
    <td>
      <a :href="url" target="_blank">twitch.tv/{{ stream.username }}</a>
    </td>
    <td>
      <span :class="`${stateStyle} px-3 py-1 rounded`">{{ stateText }}</span>
    </td>
    <td>
      <nuxt-link
        class="inline-flex shadow items-center justify-center hover:bg-gray-100 transition rounded h-10 w-10 text-gray-700"
        :to="editStreamLink"
      >
        <Icon name="edit" class="h-5" />
      </nuxt-link>
    </td>
  </tr>
</template>

<script>
export default {
  props: ['stream'],

  computed: {
    stateStyle() {
      return this.stream.is_live
        ? 'bg-s-teal-200 text-s-teal-800'
        : 'bg-red-200 text-red-800'
    },

    stateText() {
      return this.stream.is_live ? 'Online' : 'Offline'
    },

    url() {
      return `https://www.twitch.tv/${this.stream.username}`
    },

    editStreamLink() {
      return `/streams/${this.stream.id}/edit`
    },
  },
}
</script>
