<template>
<div id="sentence-completion">

<div class="question-container">

<div class="question-header">

<h2 class="question-title">
{{title}}
</h2>

<p class="question-subtitle">
{{subtitle}}
</p>

</div>

<div class="sentence-box">

<span>{{beforeSentence}}</span>

<div class="dropdown">

<div
class="dropdown-selected"
@click="toggleDropdown"
>

<span v-if="selectedOption==-1">
בחרו תשובה
</span>

<span v-else>
{{options[selectedOption]}}
</span>

<span class="arrow">
{{dropdownOpen ? "▲" : "▼"}}
</span>

</div>

<div
v-if="dropdownOpen"
class="dropdown-list"
>

<div
v-for="(option,index) in options"
:key="index"
class="dropdown-item"
@click="chooseOption(index)"
>

{{option}}

</div>

</div>

</div>

</div>

<button
class="submit-btn"
v-if="!submitted"
@click="submitAnswer"
:disabled="selectedOption==-1"
>

הגש

</button>

<button
class="move-on"
v-if="canContinue"
@click="handleMoveOn"
>

המשך

</button>

<div
class="popup-overlay"
v-if="showPopup"
>

<div class="popup">

<button
class="close-btn"
@click="closePopup"
>

✕

</button>

<h3 v-if="isCorrect">
✔ תשובה נכונה!
</h3>

<h3 v-else>
התשובה הנכונה
</h3>

<p>

{{correctSentence}}

</p>

</div>

</div>

</div>

</div>
</template>

<script>

import json from "../../text.json";

export default{

name:"SentenceCompletion",

props:["questionNum"],

data(){

return{

selectedOption:-1,

dropdownOpen:false,

submitted:false,

showPopup:false,

canContinue:false,

isCorrect:false,

points:0

}

},

computed:{

currentQuestion(){

return json.SentenceCompletion[this.questionNum];

},

title(){

return this.currentQuestion
?this.currentQuestion.title
:"";

},

subtitle(){

return this.currentQuestion
?this.currentQuestion.subtitle
:"";

},

beforeSentence(){

return this.currentQuestion
?this.currentQuestion.sentence
:"";

},

options(){

return this.currentQuestion
?this.currentQuestion.options
:[];

},

correct(){

return this.currentQuestion
?this.currentQuestion.correct
:0;

},

correctSentence(){

return this.currentQuestion
?this.currentQuestion.correctSentence
:"";

}

},

watch:{

questionNum(){

this.resetQuestion();

}

},
methods:{

toggleDropdown(){

if(this.submitted)return;

this.dropdownOpen=!this.dropdownOpen;

},

chooseOption(index){

if(this.submitted)return;

this.selectedOption=index;

this.dropdownOpen=false;

},

submitAnswer(){

this.submitted=true;

this.isCorrect=this.selectedOption===this.correct;

this.points=this.isCorrect?10:0;

this.showPopup=true;

},

closePopup(){

this.showPopup=false;

this.canContinue=true;

},

handleMoveOn(){

this.$emit("points-updated",this.points);

setTimeout(()=>{

this.$emit("next-question");

},180);

},

resetQuestion(){

this.selectedOption=-1;

this.dropdownOpen=false;

this.submitted=false;

this.showPopup=false;

this.canContinue=false;

this.isCorrect=false;

this.points=0;

}

}

}

</script>
<style scoped>

#sentence-completion{
  margin-top:20%;
  width:100%;
  display:flex;
  justify-content:center;
  direction:rtl;
  font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif;
}

.question-container{
  width:100%;
  max-width:400px;
  padding:20px;
  box-sizing:border-box;
  display:flex;
  flex-direction:column;
  align-items:center;
}

.question-header{
  text-align:center;
  height:180px;
  display:flex;
  flex-direction:column;
  justify-content:center;
}

.question-title{
  color:#5c2d13;
  font-size:28px;
  font-weight:bold;
  margin:0 0 10px;
}

.question-subtitle{
  color:#6d3b1e;
  font-size:24px;
  line-height:1.4;
  margin:0;
}

.sentence-box{
  margin-top:20px;
  width:100%;
  text-align:center;
  color:#5c2d13;
  font-size:22px;
  font-weight:bold;
  line-height:2;
}

.dropdown{
  width:100%;
  margin-top:18px;
}

.dropdown-selected{
  background:rgba(228,167,132,.95);
  border-radius:18px;
  padding:18px;
  color:white;
  font-size:20px;
  font-weight:bold;
  display:flex;
  justify-content:space-between;
  align-items:center;
  cursor:pointer;
  box-shadow:0 4px 10px rgba(92,45,19,.15);
  transition:.2s;
}

.dropdown-selected:active{
  transform:scale(.97);
}

.arrow{
  font-size:18px;
}

.dropdown-list{
  width:100%;
  margin-top:8px;
  background:white;
  border-radius:18px;
  overflow:hidden;
  box-shadow:0 6px 15px rgba(0,0,0,.2);
  animation:openDropdown .2s ease;
}

.dropdown-item{
  padding:18px;
  font-size:19px;
  font-weight:bold;
  color:#5c2d13;
  cursor:pointer;
  transition:.15s;
  border-bottom:1px solid #ececec;
}

.dropdown-item:last-child{
  border-bottom:none;
}

.dropdown-item:active{
  background:#edd2c2;
}

.submit-btn,
.move-on{
  margin-top:30px;
  background:#b97d57;
  color:white;
  font-size:22px;
  font-weight:bold;
  border:none;
  border-radius:16px;
  padding:20px 35px;
  cursor:pointer;
  box-shadow:0 4px 10px rgba(0,0,0,.18);
  transition:.18s;
  -webkit-tap-highlight-color:transparent;
}

.submit-btn:disabled{
  opacity:.5;
  cursor:not-allowed;
}

.submit-btn:active,
.move-on:active{
  transform:scale(.93);
  background:#8f5d3e;
}

.popup-overlay{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.45);
  display:flex;
  justify-content:center;
  align-items:center;
  z-index:999;
}

.popup{
  width:85%;
  max-width:420px;
  background:white;
  border-radius:25px;
  padding:30px;
  text-align:center;
  position:relative;
  animation:popup .25s ease;
}

.popup h3{
  color:#5c2d13;
  font-size:28px;
  margin-bottom:20px;
}

.popup p{
  color:#444;
  font-size:22px;
  line-height:1.7;
}

.close-btn{
  position:absolute;
  top:15px;
  left:15px;
  width:38px;
  height:38px;
  border:none;
  border-radius:50%;
  background:#b97d57;
  color:white;
  font-size:22px;
  cursor:pointer;
}

.close-btn:active{
  background:#8f5d3e;
}

@keyframes popup{
  from{
    transform:scale(.8);
    opacity:0;
  }
  to{
    transform:scale(1);
    opacity:1;
  }
}

@keyframes openDropdown{
  from{
    opacity:0;
    transform:translateY(-8px);
  }
  to{
    opacity:1;
    transform:translateY(0);
  }
}

</style>
