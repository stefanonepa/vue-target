<template>
  <circle :cx="center" :cy="center" :r="containerSize*percentage"
          :fill="fillColor" :stroke="stroke" stroke-width="0.2"
          @mouseup.stop="pull($event)" @mousemove.stop="updateChoose($event)" />
</template>
<script>
  export default {
    name: 'mycircle',
    props: ['fillColor', 'containerSize', 'val', 'percentage', 'stroke'],
    computed: {
      center () {
        return this.containerSize / 2
      }
    },
    methods: {
      pull (e) {
        this.$emit('pull', {circle: { val: this.val }, e})
      },
      updateChoose (e) {
        this.$emit('updateChoose', {circle: { val: this.val }})
      }
    }
  }
</script>
