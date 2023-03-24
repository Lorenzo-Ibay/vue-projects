<template>
  <div class="container">
    <div id="title">
        <img src="../assets/pomudachi.webp" alt=""><h1>🍂 PomuDoro 🍂</h1> <img src="../assets/pomudachi.webp" alt="">
    </div>
    <div class="timer">
        <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="first-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M165.16,163.38A172,172,0,0,0,0,0"
        />
        <path
          id="top-right"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M165.16,163.38A172,172,0,0,0,0,0"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="second-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,163.34A172,172,0,0,0,164.44,0"
        />
        <path
          id="bottom-right"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,163.34A172,172,0,0,0,164.44,0"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="third-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,0A172,172,0,0,0,165.16,162.61"
        />
        <path
          id="bottom-left"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,0A172,172,0,0,0,165.16,162.61"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="fourth-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M160.17,0A172,172,0,0,0,0,161.51"
        />
        <path
          id="top-left"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M160.17,0A172,172,0,0,0,0,161.51"
        />
      </svg>
      <div class="centerDisplay">
        <p v-if="resting">Rest</p>
        <h2>{{timeDisplay}}</h2>
      </div>
    </div>
    <button @click="startClick">{{ buttonText }}</button>
  </div>

</template>

<script>
import ProgressBar from 'progressbar.js';
import Dogmu from "../assets/Dogmu.mp3";
import amPomu from "../assets/amPomu.mp3";

export default {
    name: "homeComponent",
    data: function(){
      const workSplit = .05 * 60;
      // const breakSplit = 5;
      return{
        workSplit:workSplit,
        restSplit: .05 * 60,
        currentTimeSec:workSplit,
        currentSegment: 1,
        buttonText: "Start!",
        topRight:null,
        bottomRight:null,
        bottomLeft:null,
        topLeft:null,
        pathOptions: {
        easing: 'linear',
        duration: (workSplit * 1000) + 500
        },
        interval: null,
        alert: new Audio(Dogmu),
        restAlert: new Audio(amPomu),
        resting: false,
      };
    },
    mounted: function() {
      this.topRight = new ProgressBar.Path("#top-right", this.pathOptions);
      this.topRight.set(1);
      this.bottomRight = new ProgressBar.Path("#bottom-right",this.pathOptions);
      this.bottomRight.set(1);
      this.bottomLeft = new ProgressBar.Path("#bottom-left",this.pathOptions);
      this.bottomLeft.set(1);
      this.topLeft = new ProgressBar.Path("#top-left", this.pathOptions);
      this.topLeft.set(1);

    },
    methods: {
      startClick(){
        if(this.buttonText === "Start!" || this.buttonText === "Resume"){
          this.buttonText = "Pause";
          this.animateBar();
        } else if (this.buttonText === "Pause"){
          this.pauseBar();
          this.buttonText = "Resume";
        }
      },
      animateBar(){
        this.reduceTime();
        let segment = null;
        switch(this.currentSegment){
          case 1: segment = this.topRight; break;
          case 2: segment = this.bottomRight; break;
          case 3: segment =  this.bottomLeft; break;
          case 4: segment =  this.topLeft; break;
        }
        segment.animate( 0, {duration: (this.currentTimeSec + 1) * 1000}, this.onFinish);
      },
      pauseBar(){
        clearInterval(this.interval);
        switch(this.currentSegment){
          case 1:this.topRight.stop(); break;
          case 2:this.bottomRight.stop();break;
          case 3:this.bottomLeft.stop();break;
          case 4:this.topLeft.stop();break;
        }
      },
      onFinish(){
       if(this.currentTimeSec <= 0){
        clearInterval(this.interval);
        if(this.currentSegment <4 ){
          this.currentSegment += 1;
        }else {
          this.topRight.set(1);
          this.topLeft.set(1);
          this.bottomRight.set(1);
          this.bottomLeft.set(1);

          this.currentSegment = 1;
        }
        this.alert.play();
        this.resting= true;
        this.buttonText = "Rest";
        setTimeout(() => {
          this.buttonText = "Start!";
        this.currentTimeSec = this.restSplit;
        this.startRest();
        }, 3500);
       }
      },
      reduceTime(){
        this.interval = setInterval(() => {
          if(this.currentTimeSec > 0){
            this.currentTimeSec -= 1;
          }
        }, 1000);
      },
      startRest(){
        this.reduceTime();
        setTimeout(() => {
          clearInterval(this.interval);
          this.restAlert.play();
          this.currentTimeSec = this.workSplit;
          this.buttonText = "Start!"
          this.resting = false;
        }, this.restSplit * 1000);
      }
    },
    computed: {
      timeDisplay(){
        const minutes= parseInt(this.currentTimeSec / 60);
        const seconds= this.currentTimeSec % 60;
        const paddedMinutes= ("0" + minutes).slice(-2);
        const paddedSeconds= ("0" + seconds).slice(-2);
        return `${paddedMinutes}:${paddedSeconds}`;
      },
    }
};
</script>

<style>
.container {
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
}
#title{
    margin-top: 50px;
    display: flex;
    flex-direction: row;
    align-items: center;
}
h1{
    font-size: 64px;
    color: #c44747;
    margin-right: 10px;
    margin-left: 10px;
}
h2{
  font-size: 64px;
  color: #f85959;
}
p{
  font-size: 48px;
  line-height: 48px;
  text-align: center;
  color: #ff8080;

}
.centerDisplay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.timer{
    position: relative;
    margin-top: 100px;
    width: 330px;
    height: 330px;
}
#first-segment{
  position: absolute;
  top: 0;
  right: 0;
}
#second-segment{
  position: absolute;
  bottom: 0;
  right: 0;
}#third-segment{
  position: absolute;
  bottom: 0;
  left: 0;
}#fourth-segment{
  position: absolute;
  top: 0;
  left: 0;
}
button{
    margin-top: 50px;
    width: 200px;
    height: 68px;
    background: #f85959;
    border-radius: 20px;
    border: none;
    cursor: pointer;

    font-size: 36px;
    line-height: 45px;
    text-align: center;
    color: #fff8ee;

}
button:focus{
    outline: none;
}

</style>