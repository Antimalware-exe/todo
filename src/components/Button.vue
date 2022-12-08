<template>
  <button class="btn" @click="onClick" v-bind:style="{backgroundColor:color}">{{ text }}</button>
</template>
<script>
export default {
  name: "Button",
  props: {
    eventName: String,
    text: String,
    color: String
  },
  methods: {
    onClick() {
      this.$emit(this.eventName)
    }
  }
}</script>