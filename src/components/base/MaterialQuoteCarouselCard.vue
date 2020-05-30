<template>
  <base-material-card
    :icon="icon"
    class="v-card--material-stats"
    v-bind="$attrs"
    v-on="$listeners"
  >
    <template v-slot:after-heading>
      <div class="ml-auto text-right">
        <div
          class="body-3 grey--text font-weight-light"
          v-text="title"
        />

        <h3 class="display-2 font-weight-light text--primary">
          {{ value }} <small>{{ smallValue }}</small>
        </h3>
      </div>
    </template>

    <v-col
      cols="12"
      class="px-0"
    >
      
 <v-carousel
    cycle
    height="200"
    hide-delimiter-background
    show-arrows-on-hover
  >
    <v-carousel-item
      v-for="(comment, i) in comments"
      :key="i"
    >
      <v-sheet
        :color="slidecolors[i]"
        height="100%"
      >
        <v-row
          class="fill-height mx-6"
          align="center"
          justify="center"
          
        >
         <div> <q class="display-3"><i>{{ comment | truncate(90, '...')}}</i></q>
         <div class="display-2 pt-0 pl-12"> - {{commentnames[i]}}</div></div>
        </v-row>
      </v-sheet>
    </v-carousel-item>
  </v-carousel>

    </v-col>
    <v-col
      cols="12"
      class="px-0"
    >
      <v-divider />
    </v-col>

    <v-icon
      :color="subIconColor"
      size="16"
      class="ml-2 mr-1"
    >
      {{ subIcon }}
    </v-icon>

    <span
      :class="subTextColor"
      class="caption grey--text font-weight-light"
      v-text="subText"
    />
  </base-material-card>
</template>

<script>
  import Card from './Card'

  export default {
    name: 'MaterialStatsCard',

    inheritAttrs: false,

    props: {
      ...Card.props,
      icon: {
        type: String,
        required: true,
      },
      subIcon: {
        type: String,
        default: undefined,
      },
      subIconColor: {
        type: String,
        default: undefined,
      },
      subTextColor: {
        type: String,
        default: undefined,
      },
      subText: {
        type: String,
        default: undefined,
      },
      title: {
        type: String,
        default: undefined,
      },
      value: {
        type: String,
        default: undefined,
      },
      smallValue: {
        type: String,
        default: undefined,
      },
      comments: {
        type: Array,
        default: undefined,
      },
      commentnames: {
        type: Array,
        default: undefined,
      },
      slidecolors: {
        type: Array,
        default: undefined,
      },
      personname: {
        type: String,
        default: undefined,
      }
    },
  }
</script>

<style lang="sass">
.v-card--material-stats
  display: flex
  flex-wrap: wrap
  position: relative

  > div:first-child
    justify-content: space-between

  .v-card
    border-radius: 4px
    flex: 0 1 auto

  .v-card__text
    display: inline-block
    flex: 1 0 calc(100% - 120px)
    position: absolute
    top: 0
    right: 0
    width: 100%

  .v-card__actions
    flex: 1 0 100%
</style>
