<template>
  <Transition
    :enter-active-class="activeClass"
    :leave-active-class="activeClass"
    :enter-class="enterLeaveToClass"
    :leave-to-class="enterLeaveToClass"
    v-bind="$attrs"
    v-on="$listeners"
  >
    <slot />
  </Transition>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    activeClass: {
      type: String,
      required: true
    },
    enterLeaveToClass: {
      type: String,
      required: true
    }
  }
}
</script>
