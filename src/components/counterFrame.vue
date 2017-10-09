<template>
  <p class="counter-frame">{{timer.hrs + ':' + timer.minutes + ':' + timer.seconds}}</p>
</template>

<script>
  export default {
    name: 'CounterFrame',
    props: ['timer']
  }
</script>