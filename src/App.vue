<template>
  <div id="app">
    <header>
      <p>"may all the beings of the world be happy"</p>
      लोकाः समस्ताः सुखिनो भवन्तु
    </header>
    <main>
      <div>
        <div class="col-xs-4 text-center">
          <NumberButton number="1" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="2" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="3" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="4" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="5" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="6" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="7" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="8" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="9" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <ActionButton v-bind:actionClass="isCounting ? 'pause' : 'play'" v-bind:clickHandler="clickAction"></ActionButton>
        </div>
        <div class="col-xs-4 text-center">
          <NumberButton number="0" v-bind:clickHandler="clickNumber"></NumberButton>
        </div>
        <div class="col-xs-4 text-center">
          <ActionButton actionClass="stop" v-bind:clickHandler="clickAction"></ActionButton>
        </div>
        <div class="col-xs-12 text-center">
          <CounterFrame v-bind:timer="isRaw ? rawTime : liveCounter"></CounterFrame> 
        </div>
      </div>
    </main>
  </div>

</template>

<script>
import ActionButton from './components/actionButton'
import NumberButton from './components/numberButton'
import CounterFrame from './components/counterFrame'

class TimerDisplay {
  hrs = '00'
  minutes ='00'
  seconds ='00'
  reset = function () {
    this.hrs = '00'
    this.minutes = '00'
    this.seconds = '00'
  }
}

export default {
  name: 'app',
  components: {
    ActionButton,
    NumberButton,
    CounterFrame
  },
  data () {
    return {
      current: '',
      rawTime: new TimerDisplay(),
      liveCounter: new TimerDisplay(),
      total: 0,
      isRaw: true,
      isCounting: false,
      elapsed: 0
    }
  },
  methods: {
    clickNumber (number) {
      var self = this
      if(!self.isRaw){
        return;
      }
      if(self.current.length == 6){
        self.current = self.current.substr(1,5) + number
      }
      else{
        self.current = self.current + number.toString()
      }
      
      self.rawTime = self.calculateRaw(self.current)
      self.total = self.getTotal()
    },
    getTotal (){
      var self = this
      var time = parseInt(self.rawTime.hrs) * 60 * 60
      time += parseInt(self.rawTime.minutes) * 60
      time += parseInt(self.rawTime.seconds)

      return time
    },
    clickAction (action){
      var self = this
      if(action === "play" && self.total > 0){
        self.timerInterval = setInterval(function(){
          self.count()
        }, 1000)

        self.count();
      }
      else if(action === "pause"){
        self.stopCounting();
      }
      else if(action === "stop"){
        self.reset();
      }
    },
    calculateRaw (current){
      var self = this
      var time = self.rawTime

      if(current.length < 2) {
        time.seconds = '0' + current;
      }
      else if(current.length < 3) {
        time.seconds = current; 
      }   
      else if(current.length < 4) {
        time.seconds = current.substr(1,2);
        time.minutes = "0" + current.substr(0,1)
      }
      else if(current.length < 5) {
        time.seconds = current.substr(2,2);
        time.minutes = current.substr(0,2);
      }
      else if(current.length < 6) {
        time.seconds = current.substr(3,2);
        time.minutes = current.substr(1,2);
        time.hrs = "0" + current.substr(0,1);
      }
      else if(current.length === 6){
        time.seconds = current.substr(4,2);
        time.minutes = current.substr(2,2);
        time.hrs = current.substr(0,2);
      }
      return time;
    },
    calculateLive (){
      var self = this
      var counter = new TimerDisplay()
      var hours = Math.floor(self.total / (60 * 60));
      var minutes = Math.floor(self.total % (60*60)  / 60);
      var seconds = Math.ceil((self.total % (60*60) % 60));

      counter.hrs = hours > 9 ? hours : "0" + hours;
      counter.minutes = minutes > 9 ? minutes : "0" + minutes;
      counter.seconds = seconds > 9 ? seconds : "0" + seconds;

      if(self.elapsed % 180 === 0){
        self.playSound('middle_bowl.mp3');
      }

      return counter;
    },
    playSound(fileName) {
      var audio = new Audio('static/sounds/' + fileName);
      audio.play(); 
    },
    count (){
      var self = this;
      if(self.total === 0 && self.isRaw === false) {
        self.stopCounting();
        self.reset();
        self.finish();
        return;
      }

      //no timer yet
      if(self.total === 0) return;

      self.elapsed += 1;
      self.total = self.total-1;
      self.isRaw = false;
      self.liveCounter = self.calculateLive();
      self.isCounting = true;
    },
    stopCounting () {
      var self = this
      clearInterval(self.timerInterval);
      self.isCounting = false;
    },
    reset () {
      var self = this;
      self.stopCounting();
      self.rawTime.reset();
      self.liveCounter.reset();
      self.isRaw = true;
      self.total = 0;
      self.elapsed = 0;
      self.current = ''
    },
    finish() {
      this.playSound('ending_bowl.wav');
    }
  }
}

</script>

<style>
  header{
    width: 100%;
    text-align: center;
    margin-top: 20px;
    font-size: 28px;
    font-family: 'sofia';
    padding-left: 30px;
    padding-right: 30px;
  }
  main{
    margin-top: 40px;
  }
</style>
