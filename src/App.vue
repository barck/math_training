<template>
  <div id="training">
    <nav class="navbar navbar-dark bg-dark">
      <div class="container">
        <a href="/" class="headline"><h1 class="text-white">Math training. Level {{ level + 1 }}</h1></a>
      </div>
    </nav>
    <div class="progress">
      <div class="progress-bar" :style="progressStyles"></div>
    </div>
    <div class="box">

      <div class="container">
        <transition name="flip" mode="out-in">
        <app-start-screen
          v-if="state == 'start'"
          @onStart="onStart()"
          >
        </app-start-screen>
        <app-question
          v-else-if="state == 'question'"
          @success="onQuestSuccess"
          @error="onQuestError"
          :settings="levels[level]"
        >
        </app-question>
        <app-message
          v-else-if="state == 'message'"
          :type="message.type"
          :text="message.text"
          @onNext = "onNext"
        >
        </app-message>
        <app-result-screen
          v-else-if="state == 'result'"
          :stats="stats"
          @repeat="onStart"
          @nextLevel="onNextLevel"

        >
        </app-result-screen>
        <div v-else>Unknown state</div>
        </transition>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      state: 'start',
      stats: {
        success: 0,
        error: 0
      },
      message: {
        type: '',
        text: ''
      },
      questMax: 5,
        level: 0,
        levels: [
            {
                from: 10,
                to: 40,
                range: 5,
                variants: 2
            },
            {
                from: 100,
                to: 200,
                range: 20,
                variants: 4
            },
            {
                from: 500,
                to: 900,
                range: 40,
                variants: 6
            },
            {
                from: 1000,
                to: 9000,
                range: 100,
                variants: 6
            },
            {
                from: 10000,
                to: 90000,
                range: 1000,
                variants: 6
            },

        ]
    }
  },
  computed: {
    questDone(){
        return this.stats.success + this.stats.error;
    },
    progressStyles(){
        return {
            width: (this.questDone / this.questMax * 100) + '%'
        };
    }
  },
  methods: {
      onStart() {
          this.state = 'question'
          this.stats.success = 0
          this.stats.error = 0
      },
      onQuestSuccess(){
          this.state = 'message'
          this.message.text = 'Good job!'
          this.message.type = 'success'
          this.stats.success++;
      },
      onQuestError(msg){
          this.state = 'message'
          this.message.text = msg
          this.message.type = 'danger'
          this.stats.error++;
      },
      onNext() {
          if(this.questDone < this.questMax){
            this.state = 'question'
          }
          else{
            this.state = 'result'
          }
      },
      onNextLevel() {
          this.level++
          this.onStart()
      }
  }
}
</script>

<style lang="scss">
#training {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.navbar{
  margin-bottom: 20px;
}
.headline{
  &:hover{
    text-decoration: none;
  }
}
.box{
  margin: 20px 0;
}
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
.flip-enter{

}
.flip-enter-active{
  animation: flipInX 0.2s linear;
}
.flip-leave{

}
.flip-leave-active{
  animation: flipOutX 0.2s linear;
}
@keyframes flipInX{
  from{transform: rotateX(90deg)}
  to{transform: rotateX(0deg)}
}
@keyframes flipOutX{
  from{transform: rotateX(0deg)}
  to{transform: rotateX(90deg)}
}
.progress-bar{
  transition: width 1s;
}
</style>
