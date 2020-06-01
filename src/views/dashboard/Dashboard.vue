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
          :data="npsScoreChart.data"
          :options="npsScoreChart.options"
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
          :data="dataCompletedTasksChart.data"
          :options="dataCompletedTasksChart.options"
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

          <h3 class="card-title font-weight-light mt-2 ml-2">Sentiment analysis</h3>
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
          v-bind:values="[10, 50, 60]"
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
      npschart: [],
      sentimentChart: [],
      dailySalesChart: {
        data: this.npschart,
        options: {
          lineSmooth: this.$chartist.Interpolation.cardinal({
            tension: 0
          }),
          low: 0,
          high: 50, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 0
          }
        }
      },
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
            tension: 0
          }),
          low: 0,
          high: 10, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 0,
            right: 10,
            bottom: 0,
            left: 0
          }
        }
      },
      dataCompletedTasksChart: {
        data: {
          labels: ["12am", "3pm", "6pm", "9pm", "12pm", "3am", "6am", "9am"],
          series: [
            mockjson.eventFiltered.map(feedback => feedback.data.npsRating)
          ]
        },
        options: {
          lineSmooth: this.$chartist.Interpolation.cardinal({
            tension: 0
          }),
          low: 0,
          high: 10, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 0
          }
        }
      },
      emailsSubscriptionChart: {
        data: {
          labels: [
            "Ja",
            "Fe",
            "Ma",
            "Ap",
            "Mai",
            "Ju",
            "Jul",
            "Au",
            "Se",
            "Oc",
            "No",
            "De"
          ],
          series: [[542, 443, 320, 780, 553, 453, 326, 434, 568, 610, 756, 895]]
        },
        options: {
          axisX: {
            showGrid: false
          },
          low: 0,
          high: 1000,
          chartPadding: {
            top: 0,
            right: 5,
            bottom: 0,
            left: 0
          }
        },
        responsiveOptions: [
          [
            "screen and (max-width: 640px)",
            {
              seriesBarDistance: 5
            }
          ]
        ]
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
        var newArray = mockjson.eventFiltered.filter(function(el) {
          return (
            moment(el.timestamp)
              .startOf("week")
              .format("YYYY-MM-DD") === week
          );
        });

        let npsRatings = newArray.map(el => el.data.npsRating);
        let npsDataPoint = { x: new Date(week), y: mean(npsRatings) };
        this.npsScoreChart.data.series[0].data.push(npsDataPoint);

        let sentimentScores = newArray.map(el => el.data.commentSentimentScore);
        let sentimentDataPoint = {
          x: new Date(week),
          y: mean(sentimentScores)
        };
        this.sentimentChart.push(sentimentDataPoint);
      });
    }
  }
};
</script>
