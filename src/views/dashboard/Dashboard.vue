<template>
  <v-container id="dashboard" fluid tag="section">
    <v-row>
      <v-col cols="12" lg="4">
        <base-material-chart-card
          :data="npsScoreChart.data"
          :options="npsScoreChart.options"
          color="pink"
          hover-reveal
          type="Line"
        >
          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn v-bind="attrs" color="info" icon v-on="on">
                  <v-icon color="info">mdi-refresh</v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn v-bind="attrs" light icon v-on="on">
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h4 class="card-title font-weight-light mt-2 ml-2">NPS score</h4>

          <!-- <p class="d-inline-flex font-weight-light ml-2 mt-1">
            <v-icon color="green" small>mdi-arrow-up</v-icon>
            <span class="green--text">55%</span>&nbsp;
            increase in today's sales
          </p>

          <template v-slot:actions>
            <v-icon class="mr-1" small>mdi-clock-outline</v-icon>
            <span class="caption grey--text font-weight-light">updated 4 minutes ago</span>
          </template>-->
        </base-material-chart-card>
      </v-col>

      <v-col cols="12" lg="4">
        <base-material-chart-card
          :data="feedbackCountChart.data"
          :options="feedbackCountChart.options"
          color="success"
          hover-reveal
          type="Line"
        >
          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn v-bind="attrs" color="info" icon v-on="on">
                  <v-icon color="info">mdi-refresh</v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn v-bind="attrs" light icon v-on="on">
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h4 class="card-title font-weight-light mt-2 ml-2">Responses</h4>

          <!-- <p class="d-inline-flex font-weight-light ml-2 mt-1">
            <v-icon color="green" small>mdi-arrow-up</v-icon>
            <span class="green--text">55%</span>&nbsp;
            increase in today's sales
          </p>

          <template v-slot:actions>
            <v-icon class="mr-1" small>mdi-clock-outline</v-icon>
            <span class="caption grey--text font-weight-light">updated 4 minutes ago</span>
          </template>-->
        </base-material-chart-card>
      </v-col>

      <v-col cols="12" lg="4">
        <base-material-chart-card
          :data="sentimentChart.data"
          :options="sentimentChart.options"
          hover-reveal
          color="info"
          type="Line"
        >
          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn v-bind="attrs" color="info" icon v-on="on">
                  <v-icon color="info">mdi-refresh</v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn v-bind="attrs" light icon v-on="on">
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h3 class="card-title font-weight-light mt-2 ml-2">Comment sentiment analysis</h3>
          <!-- 
          <p class="d-inline-flex font-weight-light ml-2 mt-1">Last Last Campaign Performance</p>

          <template v-slot:actions>
            <v-icon class="mr-1" small>mdi-clock-outline</v-icon>
            <span class="caption grey--text font-weight-light">campaign sent 26 minutes ago</span>
          </template>-->
        </base-material-chart-card>
      </v-col>

      <v-col cols="12" lg="6">
        <base-material-infographic-counter-card
          color="info"
          icon="mdi-human-greeting"
          title="Detractors, Passives, Promoters"
          v-bind:categories="['Detractors', 'Passives', 'Promoters']"
          v-bind:values="[detractorCount, passiveCount, promoterCount]"
          v-bind:infoicons="['mdi-human-handsdown', 'mdi-human', 'mdi-human-handsup']"
          v-bind:infoiconcolours="['red', 'blue', 'green']"
          sub-icon="mdi-clock"
          sub-text="Just Updated"
        />
      </v-col>

      <v-col cols="12" lg="6">
        <base-material-quote-carousel-card
          color="purple"
          icon="mdi-comment"
          title="Latest comments"
          v-bind:comments="comments"
          v-bind:commentnames="commentNames"
          v-bind:slidecolors="colors"
          sub-icon="mdi-clock"
          sub-text="Just Updated"
          personname="Gordon"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import mockjson from "../../graphql/mock.json";
import { mean } from "mathjs";

export default {
  name: "DashboardDashboard",

  data() {
    return {
      eventFiltered: mockjson,
      weeks: null,
      detractorCount: 0,
      passiveCount: 0,
      promoterCount: 0,
      
      npsScoreChart: {
        data: {
          series: [
            {
              name: "series-1",
              data: []
            }
          ]
        },
        options: {
          axisX: {
            type: this.$chartist.FixedScaleAxis,
            divisor: 3,
            labelInterpolationFnc: function(value) {
              var moment = require("moment");
              return moment(value).format("MMM D");
            }
          },
          lineSmooth: this.$chartist.Interpolation.cardinal({
            tension: 1
          }),
          low: -100,
          high: 100, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 15,
            right: 10,
            bottom: 10,
            left: 0
          }
        }
      },
      feedbackCountChart: {
        data: {
          series: [
            {
              name: "series-1",
              data: []
            }
          ]
        },
        options: {
          axisX: {
            type: this.$chartist.FixedScaleAxis,
            divisor: 3,
            labelInterpolationFnc: function(value) {
              var moment = require("moment");
              return moment(value).format("MMM D");
            }
          },
          lineSmooth: this.$chartist.Interpolation.cardinal({
            tension: 1
          }),
          low: 0,
          chartPadding: {
            top: 15,
            right: 10,
            bottom: 10,
            left: 0
          }
        }
      },
    sentimentChart: {
        data: {
          series: [
            {
              name: "series-1",
              data: []
            }
          ]
        },
        options: {
          axisX: {
            type: this.$chartist.FixedScaleAxis,
            divisor: 3,
            labelInterpolationFnc: function(value) {
              var moment = require("moment");
              return moment(value).format("MMM D");
            }
          },
          lineSmooth: this.$chartist.Interpolation.cardinal({
            tension: 1
          }),
          low: -5,
          high: 5,
          chartPadding: {
            top: 15,
            right: 10,
            bottom: 10,
            left: 0
          }
        }
      },
      colors: [
        "indigo",
        "grey darken-4",
        "pink darken-2",
        "red lighten-1",
        "deep-purple accent-4"
      ],
      comments: mockjson.eventFiltered.map(feedback => feedback.data.comment),
      commentNames: mockjson.eventFiltered.map(feedback => feedback.data.user)
    };
  },

  mounted: function() {
    this.calculateWeeklyAverage();
  },

  methods: {
    complete(index) {
      this.list[index] = !this.list[index];
    },

    getXAxisDivisorCount(){
      return weeks.length -1;
    },

    calculateWeeklyAverage() {
      var moment = require("moment");

      //get the unique weeks
      this.weeks = [
        ...new Set(
          mockjson.eventFiltered.map(feedback =>
            moment(feedback.timestamp)
              .startOf("week")
              .format("YYYY-MM-DD")
          )
        )
      ];

      this.weeks.forEach(week => {
        var filteredWeekData = mockjson.eventFiltered.filter(function(el) {
          return (
            moment(el.timestamp)
              .startOf("week")
              .format("YYYY-MM-DD") === week
          );
        });

        let responseCount = filteredWeekData.length;
        let feedbackCountDataPoint = { x: new Date(week), y: responseCount };
        this.feedbackCountChart.data.series[0].data.push(feedbackCountDataPoint);

        // calculate average score from users. Commented out as currently unused
        // let npsRatings = filteredWeekData.map(el => el.data.npsRating);
        // let npsDataPoint = { x: new Date(week), y: mean(npsRatings) };
        // this.npsScoreChart.data.series[0].data.push(npsDataPoint);

        // Promoters give scores of 8+, detractors 0-6 https://nps-calculator.com/
        let npsPromoterCount = filteredWeekData.reduce((n, x) => n + (x.data.npsRating >= 9), 0);
        let npsDetractorCount = filteredWeekData.reduce((n, x) => n + (x.data.npsRating <= 6), 0);
        let promoterPercentage = (npsPromoterCount/responseCount) * 100;
        let detractorPercentage = (npsDetractorCount/responseCount) * 100;

        let npsScore = promoterPercentage - detractorPercentage;
        let npsDataPoint = { x: new Date(week), y: npsScore };
        this.npsScoreChart.data.series[0].data.push(npsDataPoint);
        this.detractorCount += npsDetractorCount;
        this.promoterCount +=npsPromoterCount;



        let sentimentScores = filteredWeekData.map(el => el.data.commentSentimentScore);
        let sentimentDataPoint = {
          x: new Date(week),
          y: mean(sentimentScores)
        };
        this.sentimentChart.data.series[0].data.push(sentimentDataPoint);
      });

      console.log('fart length');
      this.passiveCount = mockjson.eventFiltered.length - this.detractorCount - this.promoterCount;
    }
  }
};
</script>
