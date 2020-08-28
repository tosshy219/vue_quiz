<template>
  <div id="app">
    <h1 class="title">クイズ</h1>
    <div id="main">
      
      <!-- 問題 -->
      <div class="question" v-for="element in questions.slice(a,b)" :key="element.id" v-show="quiz">
        <p>{{element.question}}</p>
      </div>

      <!-- 選択肢 -->
      <ul class="choices" v-for="item in questions.slice(a,b)" :key="item.id">
        <li 
          v-for="choice in item.suggestions" 
          :key="choice.id" 
          @click="selectChoice(choice)"
          :class="next ? addClass(choice):''"
        >{{choice.suggestion}}</li>
      </ul>

      <!-- 次へボタン -->
      <button 
        id="next" 
        @click="nextQuestion();" 
        v-show="quiz" 
        :style="next ? 'background-color:lightsalmon;box-shadow:0 4px 0 rgb(216, 107, 63);color:white;':''"
      >次へ</button>

      <!-- 終了後登場 -->
      <h1 v-if="score_show===true && score===questions.length" class="fin">全問せいかいです！</h1>
      <h1 v-else-if="score_show" class="fin2">{{questions.length}}問のうち{{score}}問せいかいです！</h1>
      <a href="" v-if="score_show">Replay?</a>
    </div>
    <h1 v-show="quiz">現在{{b}}/{{questions.length}}問目です</h1>
  </div>
</template>

<script>
export default {
  data(){
    return{
      questions:[
        {
          question:'僕の好きな食べ物は？',
          suggestions:[
            {suggestion:'オムライス',correct:true},
            {suggestion:'リンゴ'},
            {suggestion:'レモン'},
          ]
        },
        {
          question:'僕の嫌いな食べ物は？',
          suggestions:[
            {suggestion:'スーパーカップ'},
            {suggestion:'板チョコ'},
            {suggestion:'カブトムシの幼虫',correct:true},
          ]
        },
        {
          question:'僕の好きな色は？',
          suggestions:[
            {suggestion:'ちょい薄めのオレンジ'},
            {suggestion:'緑',correct:true},
            {suggestion:'オシャレめのワインレッド'},
          ]
        },
        {
          question:'僕の好きなキャラは？',
          suggestions:[
            {suggestion:'ゴン'},
            {suggestion:'ヒソカ',correct:true},
            {suggestion:'カイト'},
          ]
        },
        {
          question:'僕の好きな映画のジャンルは？',
          suggestions:[
            {suggestion:'恋愛映画'},
            {suggestion:'ホラー映画',correct:true},
            {suggestion:'スプラッター映画'},
          ]
        },
      ],
      a:0,
      b:1,
      score:0,
      quiz:true,
      score_show:false,
      next:false,
      classOk:'correct',
      classNo:'wrong',
    }
  },
  methods:{
    shuffle(arr){
      for(let i = arr.length -1;i > 0;i--){
        const j =Math.floor(Math.random() * (i+1));
        [arr[j],arr[i]]=[arr[i],arr[j]];
      }
      return arr;
    },

    nextQuestion(){
      if(!this.next){
        return;
      }
      this.next=false;

      // 最後の問題になったらということ
      if(this.questions.length -1 == this.a){
        this.score_show=true;
        this.quiz=false;
      }
      this.a++;
      this.b++;

      this.shuffle(this.questions[this.a].suggestions);
    },
    selectChoice(e){
      
      if(this.next === true){
        return;
      }
      if(e.correct){
        this.score++;
        console.log(e)
      }
      this.next=true;

    },

    // :classでつけてる
    addClass(choice){
      if(choice.correct){
        return this.classOk;
      }else{
        return this.classNo;
      }
    }
  },
  created:function(){
    this.shuffle(this.questions);
    this.shuffle(this.questions[this.a].suggestions);
    console.log(this.questions[this.a].suggestions);

  },
  
  
}
</script>


<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}


</style>
