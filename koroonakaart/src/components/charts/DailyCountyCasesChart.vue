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
  name: "DailyCountyCasesChart",

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
    countyByDay () {
      return this.$store.state.data.countyByDay;
    }
  },

  methods: {
    getChartOptions() {
      this.chartOptions = {
        chartType: "linear",

        title: {
          text: this.$t("confirmedCasesByCounty"),
          align: "left",
          y: 5,
        },

        chart: {
          height: this.height,
          width: this.width,
          events: {
            load: function () {
              if (!this.exportSVGElements) return;
              // Buttons have indexes go in even numbers (button1 [0], button2 [2])
              // Odd indexes are button symbols
              const button = this.exportSVGElements[4];

              // States:
              // 0 - normal
              // 1 - hover
              // 2 - selected
              // 3 - disabled
              button.setState(2);
            },
            redraw: function () {
              if (!this.exportSVGElements) return;
              // Redraw seems to be async so setTimeout for the button to update state
              setTimeout(() => {
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

            customButton: {
              text: this.$t("logarithmic"),
              onclick: function () {
                this.options.chartType = "logarithmic";

                this.yAxis[0].update({
                  type: "logarithmic",
                  allowNegativeLog: true,
                });
              },
            },
            customButton2: {
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

        yAxis: {
          /* min: 0, */
          allowNegativeLog: true,
          title: {
            text: this.$t("numberOfCases"),
          },
        },

        tooltip: {
          formatter: (context) => {
            return formatTooltip(context, this.chartOptions.series, this.currentLocale, 0, true);
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

        legend: {
          enabled: true,
          layout: "horizontal",
          align: "center",
          verticalAlign: "bottom",
          y: 0,
        },

        plotOptions: {
          line: {
            /* or spline, area, series, areaspline etc.*/
            marker: {
              enabled: false,
            },
          },
          series: {
            showInNavigator: true,
            label: {
              connectorAllowed: false,
            },
          },
        },

        series: [
          {
            name: "Harjumaa",
            data: this.countyByDay.countyByDay.Harjumaa,
            color: "#2F7ED8",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Hiiumaa",
            data: this.countyByDay.countyByDay.Hiiumaa,
            color: "#456990",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Ida-Virumaa",
            data: this.countyByDay.countyByDay["Ida-Virumaa"],
            color: "#49BEAA",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Jõgevamaa",
            data: this.countyByDay.countyByDay.Jõgevamaa,
            color: "#49DCB1",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Järvamaa",
            data: this.countyByDay.countyByDay.Järvamaa,
            color: "#EEB868",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Läänemaa",
            data: this.countyByDay.countyByDay.Läänemaa,
            color: "#7CB5EC",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Lääne-Virumaa",
            data: this.countyByDay.countyByDay["Lääne-Virumaa"],
            color: "#6684A4",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Põlvamaa",
            data: this.countyByDay.countyByDay.Põlvamaa,
            color: "#7cb5ec",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Pärnumaa",
            data: this.countyByDay.countyByDay.Pärnumaa,
            color: "#434348",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Raplamaa",
            data: this.countyByDay.countyByDay.Raplamaa,
            color: "#90ed7d",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Saaremaa",
            data: this.countyByDay.countyByDay.Saaremaa,
            color: "#f7a35c",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Tartumaa",
            data: this.countyByDay.countyByDay.Tartumaa,
            color: "#8085e9",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Valgamaa",
            data: this.countyByDay.countyByDay.Valgamaa,
            color: "#f15c80",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Viljandimaa",
            data: this.countyByDay.countyByDay.Viljandimaa,
            color: "#e4d354",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
          {
            name: "Võrumaa",
            data: this.countyByDay.countyByDay.Võrumaa,
            color: "#2b908f",
            pointStart: Date.parse(this.caseDates[0]),
            pointInterval: 24 * 3600 * 1000,
            marker: {
              symbol: "circle",
            },
          },
        ],

        responsive: {
          rules: [
            {
              condition: {
                maxWidth: 350,
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
            {
              condition: {
                maxWidth: 575,
              },

              chartOptions: {
                legend: {
                  enabled: true,
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
      this.chartOptions.title.text = this.$t("confirmedCasesByCounty");
      this.chartOptions.yAxis.title.text = this.$t("numberOfCases");
      this.chartOptions.exporting.buttons.customButton.text = this.$t(
        "logarithmic"
      );
      this.chartOptions.exporting.buttons.customButton2.text = this.$t(
        "linear"
      );
      // this.chartOptions.exporting.buttons.customButton3.text = this.$t(
      //   "active"
      // );
    },
  },
};
</script>

<style lang="scss" scoped></style>
