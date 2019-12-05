<template>
  <div ref="main" tabindex="-1">
    <div v-if="step === 0">
      <h1 class="title">{{ $prismic.richTextAsPlain(fields.title) }}</h1>
      <p>{{ $prismic.richTextAsPlain(fields.description) }}</p>

      <button type="button" v-on:click="startQuiz(0)">Start</button>
    </div>
    <div v-else-if="step === 1 && this.questions.length">
      <fieldset>
        <legend>{{ questions[0].question_text }}</legend>

        <blockquote>
          <span>Headline</span>
          <cite>Source</cite>
        </blockquote>

        <div>
          <input type="radio" id="yes" name="actions" value="yes" />
          <label for="yes">Yes</label>
        </div>
        <div>
          <input type="radio" id="no" name="actions" value="no" />
          <label for="no">Yes</label>
        </div>
      </fieldset>
    </div>
  </div>
</template>

<script>
import Vue from "vue";

export default {
  name: "Home",
  data() {
    return {
      documentId: "",
      step: 0,
      settings: {
        quiz_format: null,
        headline_design: null
      },
      fields: {
        title: null,
        description: null
      },
      frames: [],
      frameType: null,
      currentFrame: null,
      questions: [],
      headlines: []
    };
  },
  methods: {
    // Load up app settings
    // Check for intro state
    getInitContent() {
      this.$prismic.client.getSingle("application").then(document => {
        if (document) {
          this.settings.headline_design = document.data.headline_design;
          this.settings.quiz_format = document.data.quiz_format;

          this.fields.title = document.data.intro_title;
          this.fields.description = document.data.intro_text;

          // Get all the frame groups from the CMS (sets of question/headline pairs)
          this.frames = document.data.quiz_frames;

          // If there is a title present in the App content, show the intro step,
          // else go to the next step - ignoring description as it shouldn't exist without heading
          this.step = this.$prismic.richTextAsPlain(this.fields.title) ? 0 : 1;
        } else {
          this.$router.push({ name: "not-found" });
        }
      });
    },
    // Set up quiz content
    startQuiz(frame) {
      this.step = 1;

      this.currentFrame = 1;
      this.frameType = this.frames[this.currentFrame].options.type;

      this.$prismic.client
        .getByID(this.frames[this.currentFrame].options.id)
        .then(document => {
          // if it's a question group, there is only one question, find it
          // multiple headlines, find them
          if (this.frameType === "question_group") {
            this.getQuestionsById(document.data.question.id);
            document.data.headlines.forEach(h => this.getHeadlinesById(h.headline.id));

          // if it's a headline group, there is only one headline, find it
          // multiple questions, find them
          } else if (this.frameType === "headline_group") {
            this.getHeadlinesById(document.data.headline.id);
            document.data.questions1.forEach(q => this.getQuestionsById(q.question_about_headline.id));
          }
        });

      // Handle focus for keyboard once content is in place
      this.$nextTick(function() {
        this.$refs.main.focus();
      });
    },
    // To do: refactor these to be the same reusable function
    getQuestionsById(q) {
      this.$prismic.client.getByID(q).then(document => {
        this.questions.push(document.data);
      });
    },
    getHeadlinesById(h) {
      this.$prismic.client.getByID(h).then(document => {
        this.headlines.push(document.data);
      });
    }
  },
  created() {
    this.getInitContent();
  }
};
</script>

<style>
*:focus {
  border: 2px solid red;
}
</style>