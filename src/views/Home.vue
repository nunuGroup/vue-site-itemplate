<script>
//import firebase from 'firebase';
import EditableText from '@/components/EditableText.vue'

export default {
  name: 'Home',
  components: {
    EditableText
  },
  data() {
    return {
      text: {
        landingText: null,
        section2text: null
      },
      waveOffset: "120vw",
      waveLeft: true,
      options: {
        normalScrollElements: '.test-container, .modal-active, .modal-inactive, .more-work-grid, .more-work-container, .modal-images',
        loopHorizontal: false,
        scrollingSpeed: 1000,
        slidesNavigation: false,
        controlArrows: false,
        navigation: false,
        fadingEffect: true,
        navigationPosition: 'left',
        navigationTooltips: ['Home','About', 'Services', 'Social Media','Case Studies','Contact'],
        showActiveTooltip: true,
        easingcss3: 'cubic-bezier(0.65, 0, 0.35, 1)', //swoopy
        anchors: ['home','about', 'services', 'social_media','case_studies','contact'],
        onLeave: (origin, destination, direction) => {
          this.handleLeave(origin, destination, direction);
        },
        onSlideLeave: (origin, destination, direction) => {
          this.handleSlideLeave(origin, destination, direction);
        }
      },
      triggerUp: false,
      triggerDown: false,
      fix: false,
      activeSection: 0,
      activeSlide: 1,
      hover: false,
      currentExtra: '',
      currentPreview: '',
      currentProjectText: '',
      currentProjectTitle: '',
      scrollOverflow: false,
      context: 0,
    }
  },
  props: {
    dataRef: Object
  },
  created() {

  },
  mounted() {
    this.hideNav = false; //hide nav on landing page?
    this.fetchText();
  },
  methods: {
    fetchText() {
      this.fireFetch('text', 'rel == landingText', (docs) => {
        docs.forEach((doc) => {
          this.text.landingText = doc.data().value;
        });
      });
      this.fireFetch('text', 'rel == section2text', (docs) => {
        docs.forEach((doc) => {
          this.text.section2text = doc.data().value;
        });
      });
    },
    handleLeave(origin, destination, direction) {
      console.log('origin: ', origin);
      console.log('destination: ', destination);
      console.log('direction: ', direction);
  
      this.activeSection = destination.index;

      console.log('activeSection: ', this.activeSection);

      if(direction == 'up') {
        this.triggerUp = true;
        this.triggerDown = false;
        console.log('going up');
      }
      else {
        this.triggerDown = true;
        this.triggerUp = false;
        console.log('going down');
      }

      if(destination.index == 0) {
        console.log('on first slide');
      }
    },
    handleSlideLeave(origin, destination, direction) {
      console.clear();
      console.log('origin: ', origin);
      console.log('destination: ', destination);
      console.log('direction: ', direction);
  
      this.activeSlide = destination.index;

      console.log('activeSlide: ', this.activeSlide);

      if(direction == 'up') {
        this.triggerUp = true;
        this.triggerDown = false;
      }
      else {
        this.triggerDown = true;
        this.triggerUp = false;
      }
    },
    handleMouseEnter(i, index) {
      console.clear();
      console.log('enter' , index);
      this.hover = true;
      this.currentPreview = i.thumb;
      this.currentProjectText = i.description;
      this.currentProjectTitle = i.title;
      this.context = index;

      console.log(this.currentPreview);
    },
    handleMouseLeave(i, index) {
      console.clear();
      console.log('leave', index);

      this.hover = false;

      /*if(!this.modalActive) {}*/
    },
    handleScroll() {
      //console.log(scrollY);
      //this.bannerOffset = scrollY;
    },
    goDown() {
      this.$refs.fullpage.api.moveSectionDown()
    },
    moveRight() {
      this.$refs.fullpage.api.moveSlideRight();
    },
    moveLeft() {
      this.$refs.fullpage.api.moveSlideLeft();
    }
  }
}
</script>

<template>
  <div class="home">

    <!-- optional top layer for modals & such -->
    <!--div class="top-layer">
      <div :class="( smallEnter ? 'enter' : 'stage-left' )" class="modal-small">This is some text</div>
      <div :class="( bigEnter ? 'enter' : 'stage-left' )" class="modal-big"></div>
    </div-->

    <div class="backdrop-container">
      <!--canvas id="glscreen"></canvas-->
    </div>

    <!-- social media buttons -->
    <div class="soc-container">
      <a href="#" target="_blank"><div class="hoverable soc-button fb"></div></a>
      <a href="#" target="_blank"><div class="hoverable soc-button insta"></div></a>
      <a href="#" target="_blank"><div class="hoverable soc-button tw"></div></a>
      <a href="#" target="_blank"><div class="hoverable soc-button li"></div></a>
    </div>

    <!-- optional wave -->
    <!--div :style="'left:' + waveOffset" :class="(waveLeft ? 'wave-left' : 'wave-right' )" class="wave-panel">
        <div class="wave"></div>
    </div-->

    <!-- navigation -->
    <div id="nav" class="hoverable">
      <div class="link-list hoverable">

      </div>
    </div>

    <!-- main (fullpage.js; TODO: Configuure locomotive scroll) -->
    <full-page ref="fullpage" :options="options" id="fullpage">

      <!-- Section 1 (landing page) -->
      <section class="section landing">
        <div class="landing-container">
          <div class="landing-title"></div>
          <div class="landing-text">
            <EditableText rel="landingText" align="center" width="500px" />
          </div>
          <div @click="goDown()" class="landing-button hoverable">Learn More</div>
          <div @click="goDown()" class="arrows hoverable"></div>
        </div>
      </section>
      
      <!-- Section 2 -->
      <section class="section">
        <div class="page-container">
          <EditableText rel="section2text" align="center" width="500px" />
        </div>
      </section>

      <!-- Section 3 -->
      <section class="section">
        <div class="page-container">
          <p>sample content 2</p>
        </div>
      </section>

      <!-- Section 4 -->
      <section class="section">
        <div class="page-container">
          <p>sample content 3</p>
        </div>
      </section>

      <!-- Section 5 w slides -->
      <section class="section">
        <div class="slide">
          <div class="page-container">
            slide example 1
            <div class="bottom-actions">
              <div @click="moveRight()" class="hoverable nav-button next">Next</div>
            </div>
          </div>        
        </div>
        <div class="slide">
          <div class="page-container">
            slide example 2
            <div class="bottom-actions">
              <div @click="moveLeft()" class="hoverable nav-button prev">Prev</div>
            </div>
          </div>        
        </div>
      </section>

      <!-- Section 6 -->
      <section style="background: pink" class="section">
        <div class="page-container">
          contact information
        </div>
      </section>

    </full-page>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/global';

.sec2text {

}

.landing-text {
  margin-bottom: 40px;
  opacity: 0.4;
}

.landing-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: auto;
  padding: 0px;
  position: absolute;
  left: 0px;
  right: 0px;
  bottom: 0px;
  top: 0px;

  h1 {
    font-size: 48px;
  }
}

.landing {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.landing-title {
  background-image: url('../assets/UPRISEMGMT.png');
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  width: 500px;
  height: 112px;
}

.landing-button {
  background: white;
  height: 40px;
  width: 180px;
  color: black;
  border-radius: 100px;
  line-height: 40px;
  font-weight: normal;
  border: solid 1px white;
  transition: $quick;

  &:hover {
    background: transparent;
    color: white;
  }
}

.backdrop-container {
  background: #141414;
  position: fixed;
  height: 100vh;
  width: 100%;
  top: 0px;
  margin: 0px;
}

.page-container {
  height: 100vh;
  width: 100%;
  margin: 0px;
  padding: 0px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.bottom-actions {
  position: absolute;
  bottom: 0px;
  z-index: 9999;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  //background: gray;
  color: white;
  height: 100px;
  width: 100vw;

  .nav-button {
    background: black;
    height: 40px;
    width: max-content;
    padding: 6px 24px;
    margin-right: 36px;
    line-height: 40px;
    border-radius: $rad;
    transition: $quick;

    &:hover {
      transform: $shrink;
    }
  }
}

.arrows {
  position: relative;
  width: 30px;
  height: 30px;
  transform: scale(0.6);
  margin-top: 40px;
}

.arrows:before {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  border-left: 26.66667px solid rgba(0, 0, 0, 0.7);
  border-bottom: 26.66667px solid rgba(0, 0, 0, 0.7);
  transform: translate(26.66667px, 106.66667px) rotate(-45deg);
  animation: arrows 3s linear infinite;
}

.arrows:after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  border-left: 26.66667px solid rgba(0, 0, 0, 0.7);
  border-bottom: 26.66667px solid rgba(0, 0, 0, 0.7);
  transform: translate(53.33333px, 0px) rotate(-45deg);
  animation: arrows 3s linear infinite -1.5s;
}

@keyframes arrows {
  0% {
    border-left: 2px solid transparent;
    border-bottom: 2px solid transparent;
    transform: translate(-13.33333px, -53.33333px) rotate(-45deg);
  }
  10%, 90% {
    border-left: 2px solid transparent;
    border-bottom: 2px solid transparent;
  }
  50% {
    border-left: 2px solid rgba(255, 255, 255, 0.7);
    border-bottom: 2px solid rgba(255, 255, 255, 0.7);
    transform: translate(-13.33333px, 0px) rotate(-45deg);
  }
  100% {
    border-left: 2.66667px solid transparent;
    border-bottom: 2.66667px solid transparent;
    transform: translate(-13.33333px, 53.33333px) rotate(-45deg);
  }
}

.top-layer {
  position: fixed;
  z-index: 99999;
  height: 100vh;
  width: 100%;
  display: flex;
  pointer-events: none;
  justify-content: center;
  align-items: center;
}

.soc-container {
  display: none !important;
  position: fixed;
  top: $pad;
  right: $pad;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.wave-panel {
  background: #181818;
  position: fixed;
  height: 100vh;
  width: 100%;
  margin: 0px;
  z-index: 9;
  transition: 2s;
  pointer-events: none;

  .wave {
    height: 100vh;
    transform: translateX(0px) scale(4);
    width: 175px;
    position: absolute;
    background-image: url('../assets/wave.svg');
    background-size: contain;
    background-position-y: 50%;
    background-repeat: repeat-y;
    transition: 2s;
  }
}

.wave-left {
  .wave {
    background-position-y: 7% !important;
    //background: rgba(yellow, 0.2);
  }
}

.wave-right {
  .wave {
    background-position-y: 50% !important;
    //background: rgba(aqua, 0.2);
  }
}

.main-title {
  //background:red;
  width: 70px;
  background-image: url("../assets/emblem.png");
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  height: 70px;
  margin: 42px;
}

.hoverable {
  cursor: pointer;
}

.project-text {
  font-size: 14px !important;
}

.mwm-expanded {
  background: white;
  width: auto;
  height: 70%;
  border-radius: 18px;
  min-width: 25%;
}

.mwm-inactive {
  //background: blue;
  width: 100vw;
  height: 100vh;
  position: absolute;
  padding: 0px;
  margin: 0px;
  z-index: 999;
  transition: 1s;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  pointer-events: none;
  
  img {
    transition: 600ms;
    transform: scale(0.8);
    opacity: 0;
  }
}

.mvm-active {
  background: rgba(black, 0.8);
  width: 100vw !important;
  height: 100vh !important;
  position: absolute;
  padding: 0px;
  margin: 0px;
  z-index: 999;
  transition: 1s;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 1;
  transition-delay: 150ms;

  img {
    transition: 600ms;
    transform: scale(1);
  }
}

.modal-title {
  font-size: 36px;
}

.image-list {
  list-style: none;
  padding: 0px;
  margin: 0px;
}

.modal-text {
  //@extendanimation: flyleft 1s ease forwards 3s;
  width:300px;
  text-align: left;
}

.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  //background: purple;
  transition: 1s;
}

#nav {
  position: fixed;
  z-index: 9999;

  h1 {
    padding-left: 18px;
  }

  ul {
    list-style: none;
    text-align: left;
    font-family: 'Inconsolata', monospace;

    li {
      margin-top: 6px;
      font-size: 18px;

      &:hover {
        span {
          opacity: 1;
          padding-right: 8px;
        }
      }

      span {
        opacity: 0;
        margin-right: 12px;
        transition: 300ms;
        font-weight: bold;
      }
    }
  }

  a {
    color: white;
    text-decoration: none;
    opacity: 0.7;
    //cursor: none;

    &.router-link-exact-active {
      color: white;
      opacity: 1;
      font-weight: bold;
    }
  }
}

//grid flyin animation
.grid-item {
  background: #555;
  height: 250px;
  transition: 1.2s;

  &:hover {
    //transform: scale(0.95);
    //filter:hue-rotate(145deg);
  }

  &:nth-child(1) {
    transition-delay: 1.4s;  
  }

  &:nth-child(2) {
    transition-delay: 1.2s;  
  }

  &:nth-child(3) {
    transition-delay: 1s;  
  }

  &:nth-child(4) {
    transition-delay: 1.6s;  
  }

  &:nth-child(5) {
    transition-delay: 1.4s;  
  }

  &:nth-child(6) {
    transition-delay: 1.2s;  
  }

  &:nth-child(7) {
    transition-delay: 1.8s;  
  }

  &:nth-child(8) {
    transition-delay: 1.6s;  
  }

  &:nth-child(9) {
    transition-delay: 1.4s;  
  }
}

.modal-active {
  background: black;
  width: 100vw;
  height: 100vh;
  position: absolute;
  z-index: 999;
  transition: 600ms;
  transition-delay: 600ms;
  margin: auto;
  left: 0px;
  top: 0px;
  overflow: auto;
}

.modal-inactive {
  background: black;
  opacity: 0;
  width: 100vw;
  height: 100vh;
  position: absolute;
  z-index: 999;
  transition: 600ms;
  margin: auto;
  left: 0px;
  top: 0px;
  overflow: auto;
  pointer-events: none;
}



.blur {
  filter: blur(24px);
}

.clear {
  filter: blur(0px);
}

@media only screen and (max-width: 900px) {

}
</style>