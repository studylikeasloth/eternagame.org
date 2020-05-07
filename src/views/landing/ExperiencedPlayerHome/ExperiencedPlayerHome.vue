<template>
  <EternaPage>
    <div v-if="pageData">
      <b-container
        class="video"
        :style="{
          backgroundImage: `linear-gradient(to bottom, rgba(0, 0, 0, 0),
          rgba(0, 0, 0, 0.75)),url('${progressData[`banner-image`]}')`,
        }"
      >
        <!-- <div style="float:right" class="d-flex">
          <Progress
            :progress="progressData.progressCircles[0].number"
            :total="progressData.progressCircles[0].total"
            :name="progressData.progressCircles[0].name"
            color="#2f94d1"
          />
          <Progress
            :progress="progressData.progressCircles[1].number"
            :total="progressData.progressCircles[1].total"
            :name="progressData.progressCircles[1].name"
            color="#fac244"
          />
        </div> -->
        <p style="font-size: 42px; font-weight: bold;">{{ progressData[`banner-title`] }}</p>

        <p>
          {{ progressData[`banner-sub-title`].toUpperCase() }}
        </p>
        <b-button variant="primary" size="lg" :href="puzzleRoute + '6502927'">Enter Lab</b-button>
      </b-container>

      <h1 :style="{ fontSize: '36px', fontWeight: 'bold', marginTop: '61px' }">
        {{ $t('player-home:section1') }}
      </h1>
      <Carousel>
        <swiper-slide v-for="(item, index) in masteringEterna" :key="index">
          <QuestCard :key="item.title" v-bind="item" />
        </swiper-slide>
      </Carousel>

      <h1 :style="{ fontSize: '36px', fontWeight: 'bold', marginTop: '61px' }">
        {{ $t('player-home:section2') }}
      </h1>
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
  // @ts-ignore
  import get from 'lodash.get';
  import EternaPage from '@/components/PageLayout/EternaPage.vue';
  import PageDataMixin from '@/mixins/PageData';
  import QuestCard from '@/components/Cards/QuestCard.vue';
  import Progress from '@/components/Common/Progress.vue';
  import Carousel from '@/components/Common/Carousel.vue';
  import { SwiperSlide } from 'vue-awesome-swiper';
  import { PUZZLE_ROUTE_PREFIX } from '@/utils/constants';

  @Component({
    components: {
      EternaPage,
      QuestCard,
      Progress,
      SwiperSlide,
      Carousel,
    },
  })
  export default class ExperiencedPlayerView extends Vue {
    @Prop({}) pageData!: Object;

    private puzzleRoute: string = PUZZLE_ROUTE_PREFIX;

    redirect(path: string) {
      this.$router.push(path);
    }

    get masteringEterna() {
      return get(this.pageData, 'achievement_roadmap', [])
        .filter((p: { key: string }) => (p.key as string).includes('side_quest'))
        .filter(p => p.level === p.current_level + 1);
    }

    get newPlayerRoadMap() {
      return get(this.pageData, 'achievement_roadmap', []).filter(
        (p: { key: string }) => (p.key as string) === 'ten_tools',
      );
    }

    get progressData() {
      return {
        'banner-title': 'Optimizing the Ribosome',
        'banner-sub-title': 'Ribosome Design Challenge',
        'banner-image':
          'https://cdn.zeplin.io/5e88563a3843011f95808b2f/assets/11FA9E9F-89F8-4548-A93F-241E4D1D6362.png',
        progressCircles: [
          { name: 'Designs Submitted', number: 14276, total: 24000 },
          { name: 'My Submissions', number: 526, total: 1200 },
        ],
      };
    }
  }
</script>

<style lang="scss" scoped>
  @import '@/styles/global.scss';

  .video {
    background-position: right;
    background-repeat: no-repeat;
    object-fit: cover;
    @include media-breakpoint-up(sm) {
      height: 519px;
      padding: 31px;
      padding-top: 322px;
    }
    height: 400px;
    padding-top: 91px;
    width: 100%;
  }
</style>