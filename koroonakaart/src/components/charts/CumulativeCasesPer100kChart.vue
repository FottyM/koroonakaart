<template>
  <b-container fluid>
    <highcharts v-if="chartOptions"
                :constructor-type="'stockChart'"
                class="chart"
                :options="chartOptions">
    </highcharts>
  </b-container>
</template>

<script>
import { formatTooltip } from "../../utilities/formatTooltip";

export default {
  name: "CumulativeCasesPer100kChart",  // This chart appears to be misnamed as the data isn't cumulative.

  props: {
    height: {
      default: null,
    },
    width: {
      default: null,
    },
  },

  data() {
    return {
      chartOptions: null
    };
  },

  // Get current locale
  computed: {
    currentLocale: function () {
      return this.$i18n.locale;
    },
    loaded () {
      return this.$store.state.loaded;
    },
    caseDates () {
      return this.$store.getters.caseDates;
    },
    dataCumulativeCasesChart () {
      return this.$store.getters.dataCumulativeCasesChart;
    },
  },

  methods: {
    getChartOptions() {
      this.chartOptions = {
        chartType: "linear",
        chartFirstDate: Date.UTC(2020, 1, 25),

        chart: {
          height: this.height,
          width: this.width,
          events: {
            load: function () {
              // Buttons have indexes go in even numbers (button1 [0], button2 [2])
              // Odd indexes are button symbols
              if (!this.exportSVGElements) return;

              const button = this.exportSVGElements[4];

              // States:
              // 0 - normal
              // 1 - hover
              // 2 - selected
              // 3 - disabled
              button.setState(2);
            },
            redraw: function () {
              // Redraw seems to be async so setTimeout for the button to update state
              setTimeout(() => {
                if (!this.exportSVGElements) return;

                this.exportSVGElements[4].setState(
                  this.options.chartType === "linear" ? 2 : 0
                );
                this.exportSVGElements[2].setState(
                  this.options.chartType === "logarithmic" ? 2 : 0
                );
              }, 100);
            },
          },
        },

        title: {
          text: this.$t("cumulativeCasesPer100k"),
          align: "left",
          y: 5,
        },

        exporting: {
          buttons: {
            contextButton: {
              menuItems: [
                "viewFullscreen",
                "printChart",
                "downloadPNG",
                "downloadSVG",
                "downloadCSV",
              ],
            },

            customButton2: {
              text: this.$t("logarithmic"),
              onclick: function () {
                this.options.chartType = "logarithmic";

                this.yAxis[0].update({
                  type: "logarithmic",
                });
              },
            },
            customButton: {
              text: this.$t("linear"),
              onclick: function () {
                this.options.chartType = "linear";

                this.yAxis[0].update({
                  type: "linear",
                });
              },
            },
          },
        },

        // Show Highcharts.com link at bottom right
        credits: {
          enabled: true,
        },

        legend: {
          enabled: true,
          layout: "horizontal",
          align: "center",
          verticalAlign: "bottom",
        },

        plotOptions: {
          line: {
            /* or spline, area, series, areaspline etc.*/
            marker: {
              enabled: false,
            },
          },
          series: {
            label: {
              connectorAllowed: false,
            },
          },
        },

        navigation: {
          buttonOptions: {
            verticalAlign: "top",
            y: -15,
            theme: {
              fill: "none",
              stroke: "none",
              "stroke-width": 0,
              r: 4,

              states: {
                hover: {
                  /* fill: "#f5f5f5" */
                },
                select: {
                  fill: "none",
                  style: {
                    fontWeight: "bold",
                    textDecoration: "underline",
                  },
                },
              },
              style: {
                /* color: "#039", */
                /* fontWeight: "bold", */
                textDecoration: "none",
              },
            },
          },
        },

        xAxis: {
          type: "datetime",
          dateTimeLabelFormats: {
            day: "%Y<br>%m-%d",
            week: "%Y<br>%m-%d",
            month: "%Y-%m",
            year: "%Y",
          },
          labels: {
            style: {
              fontSize: "11px",
            },
          },
        },
        // xAxis: {
        //   categories: data.dates2
        // },

        yAxis: {
          title: {
            text: this.$t("numberOfCases"),
          },
        },

        tooltip: {
          formatter: (context) => {
            return formatTooltip(
              context,
              this.chartOptions.series,
              this.currentLocale,
              1
            );
          },
          backgroundColor: "#ffffff",
          style: {
            opacity: 0.95,
          },
          shared: true,
          split: false,
          useHTML: true,
          distance: 20,
        },

        series: [
          {
            name: this.$t("active100k"),
            color: "#2f7ed8",
            pointStart: Date.parse(this.caseDates[0]), // data.dates2 first entry to UTC
            pointInterval: 24 * 3600 * 1000, // one day
            data: this.dataCumulativeCasesChart.active100k,
          },
        ],

        responsive: {
          rules: [
            {
              condition: {
                maxWidth: 670,
              },

              chartOptions: {
                chart: { marginTop: 70 },
                navigation: {
                  buttonOptions: {
                    y: 10,
                    verticalAlign: "center",
                    theme: {
                      style: {
                        width: "70px",
                      },
                    },
                  },
                },
              },
            },
          ],
        },
      };
    }
  },

  created: function () {
      if (this.loaded) {
        this.getChartOptions();
      }
  },

  watch: {
    loaded: function () {
      this.getChartOptions();
    },
    currentLocale() {
      this.chartOptions.title.text = this.$t("cumulativeCasesPer100k");
      this.chartOptions.yAxis.title.text = this.$t("numberOfCases");
      this.chartOptions.series[0].name = this.$t("active100k");
      this.chartOptions.exporting.buttons.customButton.text = this.$t("linear");
      this.chartOptions.exporting.buttons.customButton2.text = this.$t(
        "logarithmic"
      );
    },
  },
};
</script>

<style lang="scss" scoped></style>
