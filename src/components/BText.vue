<template>
   <span :style="{ backgroundColor, color, fontSize: fontSize + 'px' }" @click="handleClick">
    <slot />
  </span>
</template>


<script>
export default {
  name: 'BText',
  props: {
    id: { type: Number, required: true },
    color: { type: String, default: '#ffffff' },
    backgroundColor: { type: String, default: '#ffffff' },
    fontSize: { type: Number, default: 32 },
  },
  methods: {
    handleClick() {
      this.$emit('choose:element', this.id)
    },
  },
}
</script>

<style scoped></style>
