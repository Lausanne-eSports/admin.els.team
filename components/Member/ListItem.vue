<template>
  <tr class="h-16 border-b border-neutral-200">
    <td>{{ member.nickname }}</td>
    <td>{{ fullname }}</td>
    <td>
      <nuxt-link
        class="inline-flex shadow items-center justify-center hover:bg-gray-100 transition rounded h-10 w-10 text-gray-700"
        :to="editMemberLink"
      >
        <Icon name="edit" class="h-5" />
      </nuxt-link>
    </td>
  </tr>
</template>

<script>
export default {
  props: ['member'],

  computed: {
    editMemberLink() {
      return `/members/${this.member.id}/edit`
    },

    fullname() {
      return `${this.member.firstname || ''} ${this.member.lastname ||
        ''}`.trim()
    },
  },
}
</script>

