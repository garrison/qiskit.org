<template>
  <section class="the-learning-resources-list">
    <div class="bx--grid">
      <h2>Learning resources</h2>
      <div class="bx--row">
        <div class="bx--col-md-5 bx--col-lg-8 bx--col-xlg-7 bx--col-max-6">
          <p>
            The below are designed and created by the Qiskit team. However, we
            recommend a familiarity with
            <AppLink url="https://www.khanacademy.org/math/linear-algebra">
              linear algebra
            </AppLink>
            and
            <AppLink url="https://www.coursera.org/specializations/python">
              Python
            </AppLink>
            from these trusted resources.
          </p>
        </div>
      </div>
      <client-only>
        <cv-tabs
          class="the-learning-resources-list__filter-level"
          aria-label="navigation tab label"
          no-default-to-first
          @tab-selected="selectTopFilter"
        >
          <cv-tab
            v-for="filter in topFilters"
            :key="filter"
            :label="filter"
            :selected="filter === activeTopFilter"
          />
        </cv-tabs>
      </client-only>
      <AppFiltersResultsLayout>
        <template slot="filters-on-m-l-screen">
          <AppFieldset label="Time to spend learning">
            <client-only>
              <cv-radio-group vertical>
                <cv-radio-button
                  v-for="filter in asideFilters"
                  :key="filter"
                  name="aside-filter"
                  :value="filter"
                  :label="filter"
                  :checked="filter === activeAsideFilter"
                  :aria-checked="filter === activeAsideFilter"
                  @change="selectAsideFilter(filter)"
                />
              </cv-radio-group>
            </client-only>
          </AppFieldset>
        </template>
        <template slot="filters-on-s-screen">
          <client-only>
            <cv-dropdown
              :value="activeAsideFilter"
              @change="selectAsideFilter($event)"
            >
              <cv-dropdown-item
                v-for="filter in asideFilters"
                :key="filter"
                :value="filter"
              >
                {{ filter }}
              </cv-dropdown-item>
            </cv-dropdown>
          </client-only>
        </template>
        <template slot="results">
          <slot />
        </template>
      </AppFiltersResultsLayout>
    </div>
  </section>
</template>

<script lang="ts">
import Vue from 'vue'
import { Component, Prop } from 'vue-property-decorator'

@Component
export default class TheLearningResourceList extends Vue {
  @Prop({ type: Array, default: () => [] }) topFilters!: string[]
  @Prop(String) activeTopFilter!: string

  @Prop({ type: Array, default: () => [] }) asideFilters!: string[]
  @Prop(String) activeAsideFilter!: string

  selectTopFilter (tabIndex: number) {
    const topFilterValue = this.topFilters[tabIndex]
    if (this.activeTopFilter === topFilterValue) {
      return
    }
    this.$emit('top-filter-changed', topFilterValue)
  }

  selectAsideFilter (asideFilterValue: string): void {
    if (this.activeAsideFilter === asideFilterValue) {
      return
    }
    this.$emit('aside-filter-changed', asideFilterValue)
  }
}
</script>

<style lang="scss" scoped>
.the-learning-resources-list {
  @include responsive-grid-bg('/images/grid/grid-left-inverted.svg', 36rem);

  background-repeat: no-repeat;
  background-position: left bottom;

  &__filter-level {
    margin-bottom: $spacing-09;

    @include mq($until: medium) {
      margin-bottom: $spacing-05;
    }
  }
}
</style>

<style lang="scss">
/**
 * TODO: Review if there is some mechanism, inside the Carbon Themes
 * framework, for applying a different theme to an specific component (#703).
 *
 * If there is not, we need some alternative way of overriding component
 * internal CSS. The following approach takes advantage of BEM methodology
 * and CSS specificity to override the internal CSS.
 */
.the-learning-resources-list {
  & .bx--dropdown,
  & .bx--dropdown-item {
    background-color: $background-color-white;

      svg {
        fill: $text-color;
      }
  }

  & .bx--dropdown-item:hover,
  & .bx--dropdown--show-selected .bx--dropdown--selected:hover {
    // To match default light theme UI hover, which is not among the Carbon
    // palette. 🤦
    background-color: #e5e5e5;
  }

  & .bx--dropdown-link,
  & .bx--dropdown-text {
    color: $text-color;
    border-top-color: #dde1e6;
  }

  & .bx--dropdown-link:hover {
    border-bottom-color: #dde1e6;
  }

  & .bx--dropdown--show-selected .bx--dropdown--selected .bx--dropdown-link {
    border-top-color: #dde1e6;
    border-bottom-color: #dde1e6;
  }

  &__filter-level {
    & a.bx--tabs__nav-link {
      color: $text-color-light;
      border-bottom-color: $border-color;

      &:focus,
      &:active {
        outline: none;
      }

      &:not(.bx--tabs__nav-item--disabled) {
        color: $text-color-light;
      }
    }

    & .bx--tabs__nav-link,
    & .bx--tabs-trigger-text {
      color: $text-color;
    }

    & .bx--tabs__nav-item {
      background-color: $background-color-white;

      &--selected:not(.bx--tabs__nav-item--disabled) .bx--tabs__nav-link {
        border-bottom-color: $border-active-color;
      }

      svg {
        fill: $text-color;
      }

      &:not(.bx--tabs__nav-item--disabled) .bx--tabs__nav-link,
      &:hover:not(.bx--tabs__nav-item--selected):not(.bx--tabs__nav-item--disabled) .bx--tabs__nav-link,
      .bx--tabs__nav-item--selected:not(.bx--tabs__nav-item--disabled) .bx--tabs__nav-link:focus,
      &:not(.bx--tabs__nav-item--selected):not(.bx--tabs__nav-item--disabled):not(.bx--tabs__nav-item--selected) .bx--tabs__nav-link:focus,
      &:not(.bx--tabs__nav-item--selected):not(.bx--tabs__nav-item--disabled):not(.bx--tabs__nav-item--selected) a.bx--tabs__nav-link:active {
        color: $text-color-light;
      }
    }

    & .bx--tabs__nav-item:hover:not(.bx--tabs__nav-item--disabled) {
      box-shadow: none;
    }

    & .bx--tabs-trigger,
    & .bx--tabs__nav-item:hover:not(.bx--tabs__nav-item--selected):not(.bx--tabs__nav-item--disabled) {
      background-color: $background-color-white;

      svg {
        fill: $text-color;
      }
    }
  }

  & .bx--radio-button__appearance {
    border-color: black;
  }
}
</style>
