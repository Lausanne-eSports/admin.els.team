<template>
  <button
    class="inline-flex shadow items-center text-red-600 hover:bg-gray-200 transition rounded bg-white px-4 py-2"
    @click="showModal = true"
  >
    Delete
    <portal to="modals" v-if="showModal">
      <delete-translation-modal
        :open="showModal"
        :translation="translation"
        @close="showModal = false"
      ></delete-translation-modal>
    </portal>
  </button>
</template>

<script>
import DeleteTranslationModal from './DeleteTranslationModal'

export default {
  props: ['translation'],

  components: { DeleteTranslationModal },

  data: () => ({
    showModal: false,
  }),
}
</script>
