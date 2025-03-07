<template>
  <header class="app-page-header">
    <div class="bx--grid">
      <div class="bx--row">
        <main class="bx--col app-page-header__main">
          <div>
            <h1 class="app-page-header__headline">
              <slot name="title" />
            </h1>
            <div class="app-page-header__description">
              <slot name="description" />
            </div>
          </div>
          <AppCta v-if="cta" v-bind="cta" />
        </main>
        <aside class="bx--col-lg-5 bx--col-md-4 app-page-header__aside">
          <div>
            <div class="app-page-header__card-title-wrapper">
              <div
                class="app-page-header__card-title"
                v-text="cardTitle"
              />
            </div>
            <slot name="card" />
          </div>
        </aside>
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import Vue from 'vue'
import { Component, Prop } from 'vue-property-decorator'
import { GeneralLink } from '~/constants/appLinks'

@Component
export default class AppPageHeaderWithCard extends Vue {
  @Prop({ type: String, required: true }) cardTitle!: string
  @Prop({ type: Object, required: true }) cta!: GeneralLink
}
</script>

<style lang="scss" scoped>
.app-page-header {
  @include responsive-grid-bg-strip("/images/grid/grid-hero-learn.svg", auto, 28rem);

  padding-top: $spacing-12;

  @include mq($until: medium) {
    padding-top: $spacing-09;
  }

  &__main {
    display: flex;
    flex-flow: column;
    gap: $spacing-05;
    justify-content: space-between;

    @include mq($until: x-large) {
      gap: $spacing-06;
    }

    @include mq($until: large) {
      gap: $spacing-09;
    }
  }

  &__headline {
    margin-top: $spacing-07;
  }

  &__description {
    margin-top: $spacing-05;

    @include mq($until: x-large) {
      margin-top: $spacing-06;
    }

    @include mq($until: large) {
      margin-top: $spacing-09;
    }

    @include mq($from: large) {
      $grid-columns: 6/11; // Number of columns that the element will use at this breakpoint.

      max-width: 100% * $grid-columns;
    }
  }

  &__aside {
    @include mq($until: medium) {
      margin-top: $spacing-09;
    }
  }

  &__card-title {
    border-bottom: 4px solid $border-color-tertiary;
    display: inline;
    padding-bottom: $spacing-02;
    padding-right: $spacing-03;

    &-wrapper {
      margin-bottom: $spacing-06;
    }
  }
}
</style>
