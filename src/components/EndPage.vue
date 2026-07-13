<template>
    <div id="end-page">
        <div class="animate__animated animate__pulse" id="main-title">הגענו לסוף! ציונך הוא: {{grade}}</div>
        <div v-if="grade>80" id="sub-title">אתה בהחלט שולט בידע</div>
         <div v-else id="sub-title">נראה שתרגול נוסף נדרש כאן..</div>
        <button @click="nextPage" id="start-btn">להתחלה</button>
        <!-- <img id="tank" src="@/assets/media/tank.png" alt="tank"> -->
    </div>
</template>



<script>

export default {
    name: "end-page",
    props: ["grade"],
    data() {
        return {
            
        }
    },
    methods: {
        nextPage() {
            this.$emit('again');
        }
    }
}
</script>

<style scoped>

#end-page {
    margin: 0%;
    height: 100%;
    width: 100vw;
    position: fixed;
    top: 0;
    right: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-image: url("@/assets/media/backgroundFirst.png");
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
}
#main-title {
    text-align: center;
    color:rgb(255, 255, 255);
    font-family: "Gan";
    font-weight: bold;
    font-size: 13vw;
    
    margin: 2%;
    margin-top: 40%;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}
#sub-title {
    text-align: center;
    color:rgb(255, 255, 255);
    font-family: "Huninn";
    font-weight: bold;
    font-size: 7vw;
    margin: 2%;
    margin-top: 10%;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

#start-btn {
  background-color: #b97d57;
  color:rgb(112, 63, 33);
  font-size: 22px;
  font-weight: bold;
  border: none;
  border-radius: 16px;
  padding: 32px 25px;
  cursor: pointer;
  font-family: Gan;
  margin: 35px auto 0;
  animation: pulse 1.6s infinite;
}
@keyframes pulse {

  0% {
    transform: scale(1);
    box-shadow: 0 6px 12px rgba(0,0,0,.25);
  }

  50% {
    transform: scale(1.08);
    box-shadow: 0 10px 22px rgba(185,125,87,.55);
  }

  100% {
    transform: scale(1);
    box-shadow: 0 6px 12px rgba(0,0,0,.25);
  }

}

/* #tank{
    position:relative;
    top:19%;
    left:-20px;
    width:120%;
    z-index:2;
    animation:
        tankDrive 2s linear forwards,
        tankShake .18s infinite;
}

@keyframes tankDrive{
    from{
       left:20px;
    }
    to{
        left:60px;
    }
}

@keyframes tankShake{
    0%,100%{
        transform:translateY(0);
    }
    50%{
        transform:translateY(1px);
    }
}

*/
</style> 