<template>
    <div id="open-page">
        <div id="main-title">את"ת מנוע וממסרת</div>
        <div id="sub-title">היום תתרגלו ותבחנו על איתור ותפעול תקלות במנוע ובממסרת, ענו על השאלות וקבלו ציון ומשוב</div>
        <button @click="nextPage" id="start-btn">בואו נתחיל </button>
    </div>
</template>



<script>
export default {
    name: 'open-page',
    props: [],
    data() {
        return {
            
        }
    },
    methods: {
        nextPage() {
            this.$emit('next-page');
        }
    }
}
</script>

<style scoped>

#open-page {
    margin: 0%;
    height: 100%;
    width: 100vw;
    position: absolute;
    top: 0;
    right: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
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
</style>