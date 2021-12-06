<template>
  <div class="bg-gradient-to-tl from-blue-900 via-blue-700 to-blue-400 h-screen">
    <TheHeader />
    <div class="p-12">
      <BackgroundImage />
    </div>
  </div>
</template>

<script>
import BackgroundImage from '@/components/BackgroundImage.vue'
import TheHeader from '@/components/TheHeader.vue'
export default { components: { BackgroundImage, TheHeader } }
</script>
