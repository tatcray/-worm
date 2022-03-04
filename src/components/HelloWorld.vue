<template>
  <div class="new" ref="wrapper" @mousemove="roundRun">

    <div class="buttonsBlock wrap">
      <button @click="createAnyRound($event, 10)" class="button ShortBuff">+10</button>
      <button @click="createAnyRound($event, 50)" class="button MiddleBuff">+50</button>
      <button @click="createAnyRound($event, 100)" class="button BigBuff">+100</button>
      <button @click="change_mode(getRainbowColor)" class="btn rainbow">rainbow!</button>
      <div @click="change_mode(getRandomColor)" class="btn random">random!</div>
      <div class="box-2" style="max-width: 180px !important;
                                margin-left: 10px">
        <div class="btn-one btn-two">
          <span @click="change_mode(getNextSadColor)">Sad rainbow(</span>
        </div>
      </div>
    </div>
<!--    <div class="slideContainer">-->
<!--      <input type="range" min="1" max="10" value="5" class="slider" id="myRange">-->
<!--    </div>-->

    <div class="round" ref="round"></div>
    <div class="mainPlace" @mousemove="roundRun"
                           @touchmove.stop.prevent="roundRun"
                           @click="createRound"></div>
  </div>
</template>

<script>
export default {
  name: 'Round',
  data() {
    return {
      arrRound: [],
      rgb: {
        step: 0,
        red: 255,
        green: 0,
        blue: 0,
        gray: 127
      },
      getNextColor(){}
    }
  },
  computed: {
    round () {
      return this.$refs.round
    }
  },
  methods: {
    roundRun(event) {
      let i = this.arrRound.length - 1
      while (i >= 0) {
        if (i === 0) {
          this.arrRound[i].style.left = this.round.style.left
          this.arrRound[i].style.top = this.round.style.top
        } else {
          this.arrRound[i].style.left = this.arrRound[i - 1].style.left
          this.arrRound[i].style.top = this.arrRound[i - 1].style.top
        }
        i--;
      }
      if (event.type === "touchmove") {
        this.round.style.left = event.touches[0].pageX - this.round.clientWidth / 2 + 'px'
        this.round.style.top = event.touches[0].pageY - this.round.clientHeight / 2 + 'px'
      } else {
        this.round.style.left = event.pageX - this.round.clientWidth / 2 + 'px'
        this.round.style.top = event.pageY - this.round.clientHeight / 2 + 'px'
      }
    },
    change_mode (mode) {
      this.getNextColor = mode
    },
    change_color(elem, dataColor){
      elem.style.backgroundColor = dataColor
    },
    createRound(e) {
      let newRound = document.createElement('div')
      newRound.style.backgroundColor = this.getNextColor()
      newRound.style.left = e.pageX - this.round.clientWidth / 2 + 'px'
      newRound.style.top = e.pageY - this.round.clientHeight / 2 + 'px'
      this.$refs.wrapper.appendChild(newRound).classList.add("newRound")
      this.arrRound.push(newRound)
      // console.log(this.arrRound)
      return this.arrRound
    },
    createAnyRound(event, times) {
      while (times) {
        this.createRound(event)
        times--
      }
    },
    getRandomColor () {
      return `rgb(${Math.ceil(Math.random() * 255)}, ${Math.ceil(Math.random() * 255)}, ${Math.ceil(Math.random() * 255)})`
    },
    getNextSadColor() {
      const step = 4
      if (this.rgb.step === 0) {
        this.rgb.gray += step
        if (this.rgb.gray >= 255) {
          this.rgb.step++
        }
      }
      else {
        this.rgb.gray -= step
        if (this.rgb.gray <= 0) {
          this.rgb.step = 0
        }
      }
      return `rgb(${this.rgb.gray}, ${this.rgb.gray}, ${this.rgb.gray})`
    },
    getRainbowColor() {
      const step = 12
      if (this.rgb.step === 0) {
        this.rgb.green += step
        if (this.rgb.green >= 255) {
          this.rgb.step++
        }
      } else if (this.rgb.step === 1) {
        this.rgb.red -= step
        if (this.rgb.red <= 0) {
          this.rgb.step++
        }
      } else if (this.rgb.step === 2) {
        this.rgb.blue += step
        if (this.rgb.blue >= 255) {
          this.rgb.step++
        }
      } else if (this.rgb.step === 3) {
        this.rgb.green -= step
        if (this.rgb.green <= 0) {
          this.rgb.step++
        }
      } else if (this.rgb.step === 4) {
        this.rgb.red += step
        if (this.rgb.red >= 255) {
          this.rgb.step++
        }
      } else {
        this.rgb.blue -= step
        if (this.rgb.blue <= 0) {
          this.rgb.step = 0
        }
      }
      return `rgb(${this.rgb.red}, ${this.rgb.green}, ${this.rgb.blue})`
    }
  },
  created() {
    this.getNextColor = this.getRainbowColor
  }
}
</script>

<style>
  html{
    background: #313133;
  }
  .round {
    position: relative;
    background: dimgray;
    border-radius: 50%;
    height: 10em;
    width: 10em;
    cursor: crosshair;
  }
  .newRound {
    top: 0;
    position: absolute;
    border-radius: 50%;
    height: 10em;
    width: 10em;
    cursor: crosshair;
  }
  .mainPlace {
    top: 0;
    position: absolute;
    width: 100vw;
    height: 100vh;
    z-index: 5;
  }
  .buttonsBlock {
    z-index: 140;
    position: absolute;
    display: flex;
    justify-content: center;
  }
  .wrap {
    padding-top: 30px;
    height: 100%;
    display: flex;
    align-items: start;
    justify-content: center;
  }

  .button {
    margin: 0 10px 0 10px;
    min-width: 150px;
    min-height: 60px;
    font-family: 'Nunito', sans-serif;
    font-size: 22px;
    text-transform: uppercase;
    letter-spacing: 1.3px;
    font-weight: 700;
    color: #313133;
    background: #4FD1C5;
    background: linear-gradient(90deg, rgba(129,230,217,1) 0%, rgba(79,209,197,1) 100%);
    border: none;
    border-radius: 200px;
    box-shadow: 12px 12px 24px rgba(79,209,197,.64);
    transition: all 0.3s ease-in-out 0s;
    cursor: pointer;
    outline: none;
    position: relative;
    padding: 10px;
  }

  .button::before {
    content: '';
    border-radius: 400px;
    min-width: calc(150px + 12px);
    min-height: calc(60px + 12px);
    border: 6px solid #00FFCB;
    box-shadow: 0 0 60px rgba(0,255,203,.64);
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    opacity: 0;
    transition: all .3s ease-in-out 0s;
  }

  .button:hover, .button:focus {
    color: #313133;
    transform: translateY(-6px);
  }

  .button:hover::before, .button:focus::before {
    opacity: 1;
  }

  .button::after {
    content: '';
    width: 30px; height: 30px;
    border-radius: 100%;
    border: 6px solid #00FFCB;
    position: absolute;
    z-index: -1;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation: ring 1.5s 1;
  }

  .button:hover::after, .button:focus::after {
    animation: none;
    display: none;
  }

  @keyframes ring {
    0% {
      width: 30px;
      height: 30px;
      opacity: 1;
    }
    100% {
      width: 300px;
      height: 300px;
      opacity: 0;
    }
  }
  .MiddleBuff {
    background: yellow;
    background: linear-gradient(90deg, rgba(255,244,94,1) 0%, rgba(255,234,76,1) 100%);
    border: none;
    box-shadow: 12px 12px 24px rgba(240,230,140,.84);
  }
  .MiddleBuff::before {
    border: 6px solid yellow;
    box-shadow: 0 0 60px rgba(240,230,70,.84);
  }
  .MiddleBuff::after {
    border: 6px solid #fcff52;
  }

  .BigBuff {
    background: red;
    background: linear-gradient(90deg, rgba(240, 77, 96, 1) 0%, rgba(235, 54, 75, 1) 100%);
    border: none;
    box-shadow: 12px 12px 24px rgba(224, 94, 109,.84);
  }
  .BigBuff::before {
    border: 6px solid red;
    box-shadow: 0 0 60px rgba(189, 47, 64,.84);
  }
  .BigBuff::after {
    border: 6px solid #ff384f;
  }

  div[class*=box] {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }


  .btn-one {
    line-height: 50px;
    height: 50px;
    text-align: center;
    width: 180px;
    cursor: pointer;
  }

  .btn {
    padding: 0 25px 0 25px;
    position: relative;
    display: inline-block;
    width: 200px;
    height: 60px;
    text-align: center;
    line-height: 60px;
    color: #fff;
    font-size: 20px;
    text-transform: uppercase;
    text-decoration: none;
    font-family: sans-serif;
    box-sizing: border-box;
    background: linear-gradient(90deg, #03a9f4, #f441a5, #ffeb3b, #03a9f4);
    background-size: 400%;
    border-radius: 30px;
    z-index: 1;
  }

  .btn:hover {
    animation: animate 8s linear infinite;
  }

  @keyframes animate {
    0% {
      background-position: 0%;
    }
    100% {
      background-position: 400%;
    }
  }

  .btn:before {
    content: "";
    position: absolute;
    top: -5px;
    right: -5px;
    bottom: -5px;
    left: -5px;
    z-index: -1;
    background: linear-gradient(90deg, #03a9f4, #f441a5, #ffeb3b, #03a9f4);
    background-size: 400%;
    border-radius: 40px;
    opacity: 0;
    transition: .5s;
  }
  .btn:hover:before {
    filter: blur(20px);
    opacity: 1;
    animation: animate 8s linear infinite;
  }

  .rainbow {
    margin: 0 10px 0 10px;
    background: linear-gradient(90deg, #9668d4, #d46868, #e5eb73, #5b9ade);
  }

  .rainbow:before {
    background: linear-gradient(90deg, #9668d4, #d46868, #e5eb73, #5b9ade);
  }

  .btn-two {
    color: #FFF;
    transition: all 0.5s;
    position: relative;
  }
  .box-2 {
    margin-top: 8px;
  }
  .btn-two span {
    z-index: 2;
    display: block;
    position: absolute;
    width: 100%;
    height: 100%;
  }
  .btn-two::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    transition: all 0.5s;
    border: 1px solid rgba(255,255,255,0.2);
    background-color: rgba(255,255,255,0.1);
  }
  .btn-two::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    transition: all 0.5s;
    border: 1px solid rgba(255,255,255,0.2);
    background-color: rgba(255,255,255,0.1);
  }
  .btn-two:hover::before {
    transform: rotate(-45deg);
    background-color: rgba(255,255,255,0);
  }
  .btn-two:hover::after {
    transform: rotate(45deg);
    background-color: rgba(255,255,255,0);
  }

  /*.slidecontainer {*/
  /*  width: 100%; !* Width of the outside container *!*/
  /*}*/

  /* The slider itself */
  .slider {
    -webkit-appearance: none;  /* Override default CSS styles */
    appearance: none;
    width: 100%; /* Full-width */
    height: 25px; /* Specified height */
    background: #d3d3d3; /* Grey background */
    outline: none; /* Remove outline */
    opacity: 0.7; /* Set transparency (for mouse-over effects on hover) */
    -webkit-transition: .2s; /* 0.2 seconds transition on hover */
    transition: opacity .2s;
  }

  /* Mouse-over effects */
  .slider:hover {
    opacity: 1; /* Fully shown on mouse-over */
  }

  /* The slider handle (use -webkit- (Chrome, Opera, Safari, Edge) and -moz- (Firefox) to override default look) */
  .slider::-webkit-slider-thumb {
    -webkit-appearance: none; /* Override default look */
    appearance: none;
    width: 25px; /* Set a specific slider handle width */
    height: 25px; /* Slider handle height */
    background: #4CAF50; /* Green background */
    cursor: pointer; /* Cursor on hover */
  }

  .slider::-moz-range-thumb {
    width: 25px; /* Set a specific slider handle width */
    height: 25px; /* Slider handle height */
    background: #4CAF50; /* Green background */
    cursor: pointer; /* Cursor on hover */
  }
  
  @media (max-width: 1156px) {
    .buttonsBlock {
      display: flex;
      flex-direction: column;
      justify-content: start;
      max-width: 145px
    }

    .rainbow, .random {
      margin-left: 10px;
    }

    .button, .btn {
      width: 145px;
      margin-top: 20px;
    }

    .box-2 {
      margin-top: 20px;
      margin-bottom: 0 !important;
    }
  }
</style>
