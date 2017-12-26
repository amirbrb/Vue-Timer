<template>
  <div>
    <span class="glyphicon glyphicon-option-vertical" @click="changeSettingsVisibility"></span>
    <div id="settings" :class="{collapse: !isVisible}">
      <div class="checkbox">
        <label>
          <input type="checkbox" ref="attentionSoundController" :checked="settingsData.isUsingAttentionSound" data-toggle="toggle" v:model="isUsingAttentionSound">Sound attention gong
        </label>
      </div>
      <div :class="{collapse: !settingsData.isUsingAttentionSound}">
        <label>Every <input ref="attentionIntervalController" class="attentionInterval" @change="changeAttentionSoundInterval" :value="settingsData.attentionInterval" type="number"> Min.</label>
      </div>
      <a class="btn btn-default btn-xs" @click="changeSettingsVisibility">ok</a>
    </div>
  </div>
</template>

<script>

export default {
  name: 'settings',
  components: {
  },
  props: ['settingsData'],
  mounted(){
    var self = this;
    var attentionSoundController = window.$(self.$refs.attentionSoundController);
    attentionSoundController.change(function(e) {
      var checked = window.$(this).prop('checked');
      self.$emit('settingsChanged', {
        field: 'isUsingAttentionSound',
        value: checked
      });
    });
  },
  data () {
    return {
      isVisible: false
    }
  },
  methods: {
    changeSettingsVisibility(){
      var self = this;
      self.isVisible = !self.isVisible;
    },
    changeAttentionSoundInterval(){
      var self = this;
      self.$emit('settingsChanged', {
        field: 'attentionInterval',
        value: self.$refs.attentionIntervalController.value
      });
    }
  }
}

</script>

<style scoped>
  #settings{
    position: absolute;
    top: 8px;
    left:40px;
    z-index: 5;
    background: #FFE4E1;
    border: 1px solid #808080;
    padding-left: 20px;
    padding-right: 20px;
  }

  .btn{
    margin-top: 5px;
    margin-bottom: 5px;
    margin-left: 200px;
    margin-right: -15px;
  }

  .glyphicon-option-vertical{
    font-size: 24px;
    cursor: pointer;
    position: absolute;
    top: 15px;
    left: 15px;
  }

  .attentionInterval{
    padding-left: 5px;
    margin-right: 5px;
    width: 80px;
  }

  label{
    font-size: 16px;
    font-family: 'Sofia';
  }
</style>
