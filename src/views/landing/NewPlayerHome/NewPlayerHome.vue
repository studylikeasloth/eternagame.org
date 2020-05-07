<template>
  <EternaPage title="">
    <div v-if="pageData">
      <b-container
        class="video"
        :style="{
          backgroundImage: `${bannerImageStyle}`,
          backgroundPosition: 'right',
        }"
      >
        <p class="page-title">
          {{ $t('player-home:banner-title') }}{{ ` ${$t('player-home:banner-together')}` }}
        </p>

        <p class="explain">
          {{ $t('player-home:banner-explain') }}
        </p>
        <a href="https://www.pbs.org/wgbh/nova/labs/lab/rna/">
          <b-button class="button" variant="primary" size="lg" style="margin-right:10px">{{
            $t('player-home:next-puzzle')
          }}</b-button>
        </a>
        <a href="https://www.pbs.org/wgbh/nova/labs/lab/rna/">
          <b-button class="button" variant="secondary" size="lg">{{
            $t('player-home:nova-labs')
          }}</b-button>
        </a>

        <div class="d-flex" style="margin-top: 22px;">
          <router-link to="/news/9818657">
            <p style="margin-right: 20px;color:white;font-weight:bold;font-size:14px">
              <i class="arrow_right"></i>{{ $t('player-home:open-vaccine') }}
            </p>
          </router-link>
        </div>
      </b-container>

      <p class="section-title">
        {{ $t('player-home:lab-access') }}
      </p>
      <img v-if="progress" :src="progress" class="player-progress-bar" />
      <Carousel>
        <swiper-slide v-for="(item, index) in newPlayerRoadMap" :key="index">
          <QuestCard :key="item.title" v-bind="item" />
        </swiper-slide>
      </Carousel>
    </div>
    <div v-else>
      <h1>{{ $t('loading-text') }}</h1>
    </div>
  </EternaPage>
</template>

<script lang="ts">
  import { Component, Vue, Mixins, Prop } from 'vue-property-decorator';
  import { RouteCallback, Route } from 'vue-router';
  import { AxiosInstance } from 'axios';
  import EternaPage from '@/components/PageLayout/EternaPage.vue';
  import PageDataMixin from '@/mixins/PageData';
  import Carousel from '@/components/Common/Carousel.vue';
  import { SwiperSlide } from 'vue-awesome-swiper';
  import PuzzleCard from '@/components/Cards/PuzzleCard.vue';
  import QuestCard from '@/components/Cards/QuestCard.vue';

  import PROGRESS_IMAGE_0 from '@/assets/progress/progress0.svg';
  import PROGRESS_IMAGE_1 from '@/assets/progress/progress1.svg';
  import PROGRESS_IMAGE_2 from '@/assets/progress/progress2.svg';
  import PROGRESS_IMAGE_3 from '@/assets/progress/progress3.svg';
  import PROGRESS_IMAGE_4 from '@/assets/progress/progress4.svg';
  import PROGRESS_IMAGE_5 from '@/assets/progress/progress5.svg';
  import PROGRESS_IMAGE_6 from '@/assets/progress/progress6.svg';
  import BANNER_IMAGE from '@/assets/home/new-player-hero.png';
  import HomeData from '../types';

  const PROGRESS_IMAGES = [
    PROGRESS_IMAGE_0,
    PROGRESS_IMAGE_1,
    PROGRESS_IMAGE_2,
    PROGRESS_IMAGE_3,
    PROGRESS_IMAGE_4,
    PROGRESS_IMAGE_5,
    PROGRESS_IMAGE_6,
  ];

  @Component({
    components: {
      EternaPage,
      Carousel,
      SwiperSlide,
      PuzzleCard,
      QuestCard,
    },
  })
  export default class NewPlayerView extends Vue {
    @Prop({}) pageData!: HomeData;

    get bannerImageStyle() {
      return `linear-gradient(to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.75)), url(${BANNER_IMAGE})`;
    }

    get newPlayerRoadMap() {
      return (
        this.pageData.achievement_roadmap
        && this.pageData.achievement_roadmap.filter(
          (p: { key: string }) => (p.key as string) === 'ten_tools',
        )
      );
    }

    get progress() {
      return (
        this.pageData.achievement_roadmap
        && PROGRESS_IMAGES[this.pageData.achievement_roadmap[0].current_level]
      );
    }
  }
</script>

<style lang="scss" scoped>
  @import '@/styles/global.scss';

  .page-title {
    font-size: 2.8rem;
    font-weight: bold;
    @include media-breakpoint-only(xs) {
      font-size: 1.5rem;
    }
  }

  .video {
    background-repeat: no-repeat;
    object-fit: contain;
    height: 441px;
    // padding: 31px;
  }

  .video-wrapper {
    background-color: $dark;
    padding-top: 10px;
    border-radius: 5px;
  }

  .section-title {
    margin: 34px 0px;
    font-size: 36px;
    font-weight: bold;
    text-align: center;
    line-height: 1.37;
    margin-top: 49px;
  }

  .player-progress-bar {
    margin: 0 auto;
    display: block;
    @include media-breakpoint-only(xs) {
      width: 100%;
      height: 100px;
    }
  }

  .explain {
    max-width: 482px;
  }

  .button {
    margin-top: 10px;
  }
</style>