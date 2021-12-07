<template>
  <div v-if="!value.isShowing">
    <span class="bg-gradient-to-tl from-red-100 to-red-200 rounded-md sm:px-6 sm:py-4 shadow hover:shadow-lg text-red-600 font-bold text-lg">
      ?
    </span>
  </div>
  <div v-else>
    <span class="bg-gradient-to-tl from-blue-900 via-blue-700 to-blue-400 rounded-md sm:px-6 sm:py-4 shadow hover:shadow-lg text-white font-bold text-lg uppercase">
      {{ value.letter }}
    </span>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Object,
      required: true
    }
  }
}
</script>
