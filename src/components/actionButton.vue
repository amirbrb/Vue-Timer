<template>
  <div class="action-button button" @click="perofrmAction">
    <i v-bind:class="['glyphicon', 'glyphicon-' + this.actionClass]"></i>
  </div>
</template>

<script>
  export default {
    name: 'ActionButton',
    data () {
      return {}
    },
    props: ['actionClass', 'clickHandler'],
    methods: {
      perofrmAction () {
        var self = this;
        self.clickHandler(self.actionClass)
      }
    }
  }
</script>