<template>
  <b-container fluid>
    <highcharts v-if="chartOptions"
                class="chart"
                :options="chartOptions">
    </highcharts>
  </b-container>
</template>

<script>
import { formatNumberByLocale } from "../../utilities/formatNumberByLocale";

export default {
  name: "TestsAgeSexDistributionChart",

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

  computed: {
    currentLocale: function () {
      return this.$i18n.locale;
    },
    loaded () {
      return this.$store.state.loaded;
    },
  },

  methods: {
    getChartOptions() {
      this.chartOptions = {
        title: {
          text: this.$t("distributionOfAgeSexTests"),
          align: "left",
          y: 5,
        },

        chart: {
          absolute: true,
          type: "bar",
          height: this.height,
          width: this.width,
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
          },
        },

        navigation: {
          buttonOptions: {
            verticalAlign: "top",
            y: -15,
          },
        },

        // Show Highcharts.com link at bottom right
        credits: {
          enabled: true,
        },

        xAxis: [
          {
            categories: [
              "0 - 4",
              "5 - 9",
              "10 - 14",
              "15 - 19",
              "20 - 24",
              "25 - 29",
              "30 - 34",
              "35 - 39",
              "40 - 44",
              "45 - 49",
              "50 - 54",
              "55 - 59",
              "60 - 64",
              "65 - 69",
              "70 - 74",
              "75 - 79",
              "80 - 85",
              "85+",
              this.$t("unknown"),
            ],
            reversed: false,
            labels: {
              step: 1,
            },
          },
          {
            opposite: true,
            reversed: false,
            categories: [
              "0 - 4",
              "5 - 9",
              "10 - 14",
              "15 - 19",
              "20 - 24",
              "25 - 29",
              "30 - 34",
              "35 - 39",
              "40 - 44",
              "45 - 49",
              "50 - 54",
              "55 - 59",
              "60 - 64",
              "65 - 69",
              "70 - 74",
              "75 - 79",
              "80 - 85",
              "85+",
              this.$t("unknown"),
            ],
            linkedTo: 0,
            labels: {
              step: 1,
            },
          },
        ],

        yAxis: {
          title: {
            text: this.$t("numberOfTests"),
          },
          labels: {
            formatter: function () {
              return Math.abs(this.value);
            },
          },
        },

        plotOptions: {
          bar: {
            stacking: "normal",
            enableMouseTracking: true,
          },
        },

        tooltip: {
          formatter: (context) => {
            var seriesName = context.chart.hoverPoint.series.name;
            var category = context.chart.hoverPoint.category;
            var color = context.chart.hoverPoint.series.color;
            var value = formatNumberByLocale(
              Math.abs(context.chart.hoverPoint.y),
              this.currentLocale,
              0
            );

            // Compose tooltip
            var tooltip = "<span style='color: " + color + "'>●</span>&nbsp;";
            tooltip += "<b>" + seriesName + " " + category + "</b><br>";
            tooltip += value;
            return tooltip;
          },
          backgroundColor: "#ffffff",
          style: {
            opacity: 0.95,
          },
          useHTML: true,
        },

        series: [
          {
            name: this.$t("maleNegative"),
            visible: false,
            data: this.$store.state.data.dataPositiveTestsByAgeChart.maleNegative.map(
              (x) => x * -1
            ),
            color: "#97beeb",
          },
          {
            name: this.$t("malePositive"),
            data: this.$store.state.data.dataPositiveTestsByAgeChart.malePositive.map(
              (x) => x * -1
            ),
            color: "#2f7ed8",
          },
          {
            name: this.$t("femaleNegative"),
            visible: false,
            data: this.$store.state.data.dataPositiveTestsByAgeChart.femaleNegative,
            color: "#917ea9",
          },
          {
            name: this.$t("femalePositive"),
            data: this.$store.state.data.dataPositiveTestsByAgeChart.femalePositive,
            color: "#492970",
          },
        ],
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
      this.chartOptions.title.text = this.$t("distributionOfAgeSexTests");
      this.chartOptions.yAxis.title.text = this.$t("numberOfTests");
      this.chartOptions.series[0].name = this.$t("maleNegative");
      this.chartOptions.series[1].name = this.$t("malePositive");
      this.chartOptions.series[2].name = this.$t("femaleNegative");
      this.chartOptions.series[3].name = this.$t("femalePositive");
    },
  },
};
</script>

<style lang="scss" scoped></style>
