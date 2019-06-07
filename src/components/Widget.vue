<template>
  <div id="brandingWidget" class="widget">

    <!-- Modal content -->
    <div class="widget-content">
      <div class="widget-header">
        <div class="logo">
          <img :src="logo" alt="Logo">
        </div>
        <span class="close">&times;</span>
      </div>
      <div class="widget-body">
        <carousel
          :per-page="1"
          :mouse-drag="false"
          :paginationPosition="'top'"
          :paginationSize="6"
          :paginationActiveColor="'#D2D2D2'"
          :paginationColor="'#f1f1f1'"
          :paginationPadding="4">
          <slide
            data-index="0"
            data-name="question1"
            @slideclick="gotoSlide(1)"
            class="widget-question"
            v-for="question in questions"
            v-bind:key="question.id"
          >
            <p>
              {{ question.title }}
            </p>
          </slide>
        </carousel>
        <div class="scale">
          <ul id="scale">
            <li @click="gotoSlide()" class="score" v-for="score in scores" v-bind:key="score.id">{{ score.rate }}</li>
          </ul>
          <span>Do not agree</span>
          <span>Totally agree</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import { Carousel, Slide } from 'vue-carousel';

export default {
  name: 'Widget',
  components: {
    Carousel,
    Slide
  },
  props: {

  },
  data() {
    return {
      questions: [
        { title: 'Season 8, episode 3 was the worst episode of Game of Thrones ever.' },
        { title: 'Khaleesi turning into the Mad Queen really ruined the whole Game of Thrones series.' },
        { title: 'If Jon Snow would have killed Khaleesi and Grey Worm, he could have rescued millions of innocent people.' },
        { title: 'There should be a Game of Thrones season 9, which shows Sansa as the queen on the throne.' },
        { title: 'If Arya would have been a bit hotter, she could have been a role model for a lot of young girls.' }
      ],
      logo: '',
      bgColor: '',
      scores: [
        { rate: '1' },
        { rate: '2' },
        { rate: '3' },
        { rate: '4' },
        { rate: '5' },
      ],
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load() {
      axios.get(process.env.VUE_APP_EMPLOYER_BRANDING_API_ENDPOINT_COMPANIES).then(response => {
        this.logo = response.data.data.logo;
        this.bgColor = response.data.data.backgroundColor
      });
      axios.get(process.env.VUE_APP_EMPLOYER_BRANDING_API_ENDPOINT_QUESTIONS).then(response => {
          this.questions = response.data.data
      });
    },
    gotoSlide() {
      let slider = window.document.getElementsByClassName('VueCarousel-inner');
      if (window.innerWidth < 810) {
        slider[0].style.transform = "translate3d(" + "-" + slider[0].style.flexBasis + ", 0px, 0px)";
      } else {
        slider[0].style.transform = "translate3d(-810px, 0px, 0px)";
      }
    }
  },
}
</script>

<style scoped>
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
  }
  .widget-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 45px;
    padding: 2px 16px;
    border-top-left-radius: 3px;
    border-top-right-radius: 3px;
    background: #05377E;
    color: white;
  }

  .widget-header span {
    font-size: larger;
  }

  .widget-header .logo {
    height: 18px;
  }

  /* Modal Body */
  .widget-body {
    padding: 2px 27px;
    display: flex;
    flex-direction: column;
  }

  .widget-question p {
    margin-top: 0;
    font-size: 14px;
    line-height: 18px;
    color: #333333;
  }

  .scale {
    position: relative;
    width: 255px;
    margin: 0 auto;
    align-self: flex-end;
  }

  .scale ul {
    display: flex;
    justify-content: center;
  }

  .scale span {
    position: absolute;
    display: block;
    font-size: 14px;
    line-height: 18px;
  }

  .scale span:last-of-type {
    right: 0;
  }

  .score {
    width: 43px;
    height: 43px;
    border: 1px solid #000000;
    box-sizing: border-box;
    border-radius: 2px;
    margin-right: 10px;
    line-height: 42px;
    text-align: center;
    cursor: pointer;
  }

  .score:last-of-type {
    margin-right: 0;
  }

  /* Modal Content */
  .widget-content {
    position: fixed;
    right: 43px;
    bottom: 0;
    width: 340px;
    height: 294px;
    animation-name: animatebottom;
    animation-duration: 0.4s;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  }

  /* Add Animation */
  @keyframes animatebottom {
    from {bottom: -400px; opacity: 0}
    to {bottom: 0; opacity: 1}
  }
</style>
