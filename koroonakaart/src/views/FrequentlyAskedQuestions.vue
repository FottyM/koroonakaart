<template>
  <div class="mb-4">
    <b-container fluid="lg" role="tablist" class="h-100">
      <h2>{{ $t("faq.faqLong") }}</h2>
      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button
            block
            href="#"
            v-b-toggle.accordion-1
            class="accordion-header"
            >{{ $t("faq.q1") }}</b-button
          >
        </b-card-header>
        <b-collapse
          id="accordion-1"
          visible
          accordion="my-accordion"
          role="tabpanel"
        >
          <b-card-body>
            <b-card-text>{{ $t("faq.a1") }}</b-card-text>
          </b-card-body>
        </b-collapse>
      </b-card>

      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button
            block
            href="#"
            v-b-toggle.accordion-2
            class="accordion-header"
            >{{ $t("faq.q2") }}</b-button
          >
        </b-card-header>
        <b-collapse id="accordion-2" accordion="my-accordion" role="tabpanel">
          <b-card-body>
            <b-card-text>{{ $t("faq.a2") }}</b-card-text>
          </b-card-body>
        </b-collapse>
      </b-card>

      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button
            block
            href="#"
            v-b-toggle.accordion-3
            class="accordion-header"
            >{{ $t("faq.q3") }}</b-button
          >
        </b-card-header>
        <b-collapse id="accordion-3" accordion="my-accordion" role="tabpanel">
          <b-card-body>
            <b-card-text>
              {{ $t("faq.a3") }}
              <a href="https://okee.ee" rel="noopener" target="_blank"
                >Open Knowledge Estonia</a
              >.
            </b-card-text>
          </b-card-body>
        </b-collapse>
      </b-card>

      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button
            block
            href="#"
            v-b-toggle.accordion-4
            class="accordion-header"
            >{{ $t("faq.q4") }}</b-button
          >
        </b-card-header>
        <b-collapse id="accordion-4" accordion="my-accordion" role="tabpanel">
          <b-card-body>
            <b-card-text>
              {{ $t("faq.a4") }}
              <a
                href="https://github.com/okestonia/koroonakaart/issues"
                rel="_noopener"
                target="_blank"
                >https://github.com/okestonia/koroonakaart/issues</a
              >.
            </b-card-text>
          </b-card-body>
        </b-collapse>
      </b-card>

      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button
            block
            href="#"
            v-b-toggle.accordion-5
            class="accordion-header"
            >{{ $t("faq.q5") }}</b-button
          >
        </b-card-header>
        <b-collapse id="accordion-5" accordion="my-accordion" role="tabpanel">
          <b-card-body>
            <b-card-text>{{ $t("faq.a5") }}</b-card-text>
          </b-card-body>
        </b-collapse>
      </b-card>

      <b-card no-body class="mb-1">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <b-button
            block
            href="#"
            v-b-toggle.accordion-6
            class="accordion-header"
            >{{ $t("faq.q6") }}</b-button
          >
        </b-card-header>
        <b-collapse id="accordion-6" accordion="my-accordion" role="tabpanel">
          <b-card-body>
            <b-card-text>{{ $t("faq.a6") }}</b-card-text>
          </b-card-body>
        </b-collapse>
      </b-card>
      <b-button
        block
        pill
        variant="outline-primary"
        class="router-link-active"
        @click="this.goBackHome"
      >
        <ArrowLeft />
        {{ $t("faq.back") }}
      </b-button>
    </b-container>
  </div>
</template>

<script>
import ArrowLeft from "vue-material-design-icons/ArrowLeft.vue";

export default {
  name: "FrequentlyAskedQuestions",

  components: {
    ArrowLeft,
  },

  methods: {
    goBackHome: function () {
      if (this.$route.path === `/${this.$i18n.locale}`) return;

      this.$store.dispatch("toggleFaqInactive");
      this.$router.push(`/${this.$i18n.locale}`);
    },
  },
};
</script>

<style lang="scss" scoped>
.accordion-header {
  background-color: #4281b9;
  color: white;
  text-align: left;
}

.card-text {
  text-align: justify;
}
.accordion-header:hover {
  background-color: rgb(65, 138, 187);
  color: rgb(251, 251, 251);
}

.router-link-active {
  border: 1px solid rgb(65, 138, 187);
  color: rgb(65, 138, 187);
  float: left;
  margin-top: 40px;
  margin-bottom: 30px;
  text-align: center;
  width: 20%;

  & > * {
    margin-right: 1em;
  }

  &:hover {
    background-color: rgb(65, 138, 187);
    color: white;
  }

  @media only screen and (max-width: 600px) {
    float: right;
    width: 40%;
  }
}
h2 {
  margin-bottom: 0.8em;
  margin-top: 0.2em;
  text-align: left;
}
</style>
