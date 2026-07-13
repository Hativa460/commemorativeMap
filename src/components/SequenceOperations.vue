<template>
<div id="sequence">

<div class="question-container">

<div class="question-header">
<h2 class="question-title">{{title}}</h2>
<p class="question-subtitle">{{subtitle}}</p>
</div>

<div class="steps">

<div
v-for="(step,index) in slots"
:key="index"
class="step-box"
:class="stepClass(index)"
@click="selectSlot(index)"
>

<span v-if="step">
{{step}}
</span>

<span v-else class="placeholder">
שלב {{index+1}}
</span>

</div>

</div>

<div class="options">

<div
v-for="(option,index) in availableOptions"
:key="option"
class="option"
:class="{selected:selectedOption===index}"
@click="selectOption(index)"
>

{{option}}

</div>

</div>

<button
class="submit-btn"
v-if="!submitted"
@click="submit"
>
הגש
</button>

<button
class="move-on"
v-if="submitted"
@click="nextQuestion"
>
המשך
</button>

</div>

</div>
</template>

<script>
import json from "../../text.json";

export default{

name:"sequence-operations",

props:["questionNum"],

data(){
return{

slots:["","","",""],

availableOptions:[],

selectedOption:null,

submitted:false,

points:0

}
},

computed:{

currentQuestion(){
return json.SequenceOperations[this.questionNum];
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

options(){
return this.currentQuestion
?this.currentQuestion.options
:[];
},

correctList(){
return this.currentQuestion
?this.currentQuestion.correctList
:[];
}

},

mounted(){

this.resetQuestion();

},

watch:{

questionNum(){

this.resetQuestion();

}

},
methods:{

resetQuestion(){

this.slots=["","","",""];

this.availableOptions=[...this.options];

this.selectedOption=null;

this.submitted=false;

this.points=0;

},

selectOption(index){

if(this.submitted)return;

this.selectedOption=index;

},

selectSlot(index){

if(this.submitted)return;

if(this.selectedOption===null){

if(this.slots[index]!=""){

this.availableOptions.push(this.slots[index]);

this.slots.splice(index,1,"");

}

return;

}

if(this.slots[index]!=""){

this.availableOptions.push(this.slots[index]);

}

this.$set(this.slots,index,this.availableOptions[this.selectedOption]);

this.availableOptions.splice(this.selectedOption,1);

this.selectedOption=null;

},

submit(){

this.submitted=true;

let score=0;

this.slots.forEach((item,index)=>{

if(item===this.correctList[index]){

score+=5;

}

});

this.points=score;

},

stepClass(index){

if(!this.submitted){

return "";

}

return this.slots[index]===this.correctList[index]
?"correct"
:"wrong";

},

nextQuestion(){

this.$emit("points-updated",this.points);

setTimeout(()=>{

this.$emit("next-question");

},180);

}

}

}
</script>
<style scoped>

#sequence{
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
  margin-bottom:20px;
  height:170px;
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

.steps{
  width:100%;
  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:15px;
  margin-top:15px;
}

.step-box{
  min-height:95px;
  border-radius:18px;
  background:rgba(228, 167, 132, 0.82);
  display:flex;
  justify-content:center;
  align-items:center;
  text-align:center;
  padding:10px;
  box-sizing:border-box;
  color:white;
  font-size:18px;
  font-weight:bold;
  cursor:pointer;
  transition:.2s;
  border:3px dashed #8b5a3c;
}

.step-box:active{
  transform:scale(.96);
}

.placeholder{
  color:rgba(255,255,255,.7);
}

.options{
  width:100%;
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:10px;
  margin-top:35px;
}

.option{
  min-height:60px;
  background:#b97d57;
  border-radius:15px;
  display:flex;
  justify-content:center;
  align-items:center;
  text-align:center;
  color:white;
  font-size:15px;
  font-weight:bold;
  cursor:pointer;
  transition:.2s;
  padding:8px;
  box-sizing:border-box;
}

.option:active{
  transform:scale(.93);
}

.option.selected{
  border:3px solid #0d47a1;
  box-shadow:0 0 14px rgba(13,71,161,.35);
}

.correct{
  border:3px solid #2e7d32 !important;
  background:rgba(179,241,160,.95);
  color:black;
}

.wrong{
  border:3px solid #c62828 !important;
  background:rgba(247,83,83,.95);
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
  transition:.15s;
  box-shadow:0 4px 10px rgba(0,0,0,.15);
  -webkit-tap-highlight-color:transparent;
}

.submit-btn:active,
.move-on:active{
  transform:scale(.93);
  background:#8f5d3e;
}

</style>