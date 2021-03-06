<template>
  <EternaPage>
    <div class="overview">
      <h2 class="overview-title">
        Eterna is a citizen science game that recruits players to design RNA molecules for
        biomedical research.
      </h2>
      <h3 class="overview-subtitle text-muted">
        Players and researchers have created original software programs to aid in complex RNA
        design, which are now available for licensing.
      </h3>
    </div>

    <Gallery :sm="12" :md="6">
      <SoftwareCard v-for="project in projects" :key="project.title" :project="project" />
    </Gallery>
  </EternaPage>
</template>

<script lang="ts">
  import { Component, Prop, Vue, Mixins } from 'vue-property-decorator';
  import { RouteCallback, Route } from 'vue-router';
  import { AxiosInstance } from 'axios';
  import DropdownSidebarPanel, { Option } from '@/components/Sidebar/DropdownSidebarPanel.vue';
  import EternaPage from '@/components/PageLayout/EternaPage.vue';
  import SoftwareCard from './SoftwareCard.vue';

  export interface SoftwareProject {
    title: string;
    logoUrl: string;
    description: string;
    // TODO: Download, Tutorial
    thumbnailUrl: string;
    paperTitle: string;
    paperUrl: string;
    journal: string;
    date: string;
  }

  const mockProject: SoftwareProject = {
    title: 'EternaBrain',
    logoUrl: 'https://software.eternagame.org/img/logo_eternabrain.svg',
    description:
      'The Eterna project has collected over 1 million player moves by crowdsourcing RNA designs in the form of puzzles that reach extraordinary difficulty. EternaBrain is a multilayer convolutional neural network trained on this player data, which surpasses all six other prior algorithms that were not informed by Eterna strategies and suggests a path for automated RNA design to achieve human-competitive performance.',
    thumbnailUrl: 'https://software.eternagame.org/img/thumb_eternabrain_paper.png',
    paperTitle:
      'EternaBrain: Automated RNA design through move sets from an Internet-scale RNA videogame',
    paperUrl: 'https://doi.org/10.1101/326736',
    journal: 'bioRxiv',
    date: 'May 2018',
  };

  const projects: SoftwareProject[] = [
    mockProject,
    mockProject,
    mockProject,
    mockProject,
    mockProject,
  ];

  @Component({
    components: {
      EternaPage,
      SoftwareCard,
    },
  })
  export default class SoftwareExplore extends Vue {
    projects: SoftwareProject[] = projects;
  }
</script>

<style scoped lang="scss">
  .overview {
    max-width: 70%;
    margin: 2rem;
    margin-bottom: 4rem;
  }

  .overview-title {
    font-size: 1.9rem;
    line-height: 2.5rem;
  }

  .overview-subtitle {
    font-size: 1.6rem;
    line-height: 2.2rem;
    font-weight: normal;
  }

  h2 {
    line-height: 4rem;
    font-size: 2rem;
    font-weight: bold;
  }
</style>
