<template>
  <div class="flex space-x-8 text-white font-semibold uppercase bg-blue-200 text-blue-600 p-2 justify-center">
    <span>
      Wins: {{ stats.wins }}
    </span>
    <span>
      Losses: {{ stats.losses }}
    </span>
  </div>
</template>

<script>
export default {
  props: {
    stats: {
      type: Object,
      required: true
    }
  }
}
</script>
