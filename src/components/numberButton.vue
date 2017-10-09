<template>
  <div class="number-button button" @click="setTime">{{number}}</div>
</template>

<script>
  export default {
    name: 'NumberButton',
    props: ['number', 'clickHandler'],
    methods: {
      setTime () {
        var self = this;
        self.clickHandler(self.number)
      }
    }
  }
</script>