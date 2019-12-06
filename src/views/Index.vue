<template>
  <main ref="main" tabindex="-1" v-cloak v-bind:class="[loading ? 'is-loading' : '', themes[currentTheme]]">
    <div class="bg" v-bind:class="'bg-step-'+step">
      <div class="bg-1">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 3386.062 468.552">
          <path
            d="M3386.062,468.552H0V0C423.258,0,423.258,113.739,846.516,113.739S1269.773,0,1693.031,0s423.258,113.739,846.516,113.739S2962.8,0,3386.062,0Z"
            fill="#f2e5ad"
          />
        </svg>
      </div>
      <div class="bg-2">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 3386.062 468.552">
          <path
            d="M3386.062,468.552H0V0C423.258,0,423.258,113.739,846.516,113.739S1269.773,0,1693.031,0s423.258,113.739,846.516,113.739S2962.8,0,3386.062,0Z"
            fill="#f2e5ad"
          />
        </svg>
      </div>
    </div>
    <div v-if="step === 0">
      <div class="main-content">
        <h1 class="title">{{ $prismic.richTextAsPlain(fields.title) }}</h1>
        <p>{{ $prismic.richTextAsPlain(fields.description) }}</p>
      </div>
      <div class="main-actions">
        <div class="main-actions-group">
          <button class="button" type="button" v-on:click="startQuiz(0)">
              <span class="icon-animated">
                  <svg class="icon-face" xmlns="http://www.w3.org/2000/svg" width="21.4" height="14.936" viewBox="0 0 21.4 14.936"><g transform="translate(0)"><path d="M27.771,52.729a14.432,14.432,0,0,1-10.244-4.236.684.684,0,0,1,.967-.967,13.137,13.137,0,0,0,18.556,0,.684.684,0,0,1,.967.967A14.44,14.44,0,0,1,27.771,52.729Z" transform="translate(-16.845 -37.793)" fill="#4d4d4d"/><path d="M21.35,29.3a2.53,2.53,0,1,0-2.53,2.53A2.53,2.53,0,0,0,21.35,29.3Z" transform="translate(-16.29 -26.77)" fill="#4d4d4d"/><path d="M54.15,26.77a2.53,2.53,0,1,0,2.53,2.53A2.53,2.53,0,0,0,54.15,26.77Z" transform="translate(-35.28 -26.77)" fill="#4d4d4d"/></g></svg>
                  <svg class="icon-head" xmlns="http://www.w3.org/2000/svg" width="37" height="37" viewBox="0 0 37 37"><circle cx="18.5" cy="18.5" r="18.5" fill="#f7b239"/><g transform="translate(0.002 3.834)" opacity="0.8"><path d="M8.784,32.669A18.5,18.5,0,0,1,7.213,8.29a18.959,18.959,0,0,0-1.79,1.572A18.5,18.5,0,0,0,31.592,36.031a18.321,18.321,0,0,0,1.572-1.79A18.5,18.5,0,0,1,8.784,32.669Z" transform="translate(-0.005 -8.29)" fill="#e09b2d"/><path d="M35.519,62.665a12.143,12.143,0,0,1-3.279-.453.682.682,0,1,1,.37-1.313,10.807,10.807,0,0,0,5.818,0,.682.682,0,0,1,.37,1.313A11.987,11.987,0,0,1,35.519,62.665Z" transform="translate(-17.021 -36.485)" fill="#e09b2d"/></g></svg>
              </span>
              Start
            </button>
        </div>
      </div>
    </div>
    <div v-else-if="step === 1 && this.questions.length">
       <!-- A headline group is the frame where a question stays the same but the headline changes -->
      <div v-if="frameType === 'headline_group'">
        <fieldset>
          <div class="main-content">
            <div class="main-content-group">
              
               <legend ref="question" class="question">
                   <transition name="bounce">
                   <div>{{ questions[current].question_text }}</div>
                   </transition>
                </legend>
              

              <blockquote class="headline" v-bind:class="[settings.headline_design]">
                <cite v-if="headlines[0].headline_source && settings.headline_design === 'is-Detailed'">{{ headlines[0].headline_source }}</cite>
                <span>{{ headlines[0].headline_text }}</span>
                <cite v-if="headlines[0].headline_source  && settings.headline_design === 'is-Simple'">{{ headlines[0].headline_source }}</cite>
                <span v-if="headlines[0].headline_source && settings.headline_design === 'is-Detailed'" class="headline-copy"></span>
              </blockquote>
              
            </div>
          </div>
          <div class="main-actions">
            <div class="main-actions-group">
                <!--                 
                Ideally these event functions should happen on change, but since selecting an option brings you to the next step,
                the change event would trigger immediately on keyboard focus (that's how radio buttons work)
                Would either need to change from a radio button (which semantically, it should be), or handle events for click, enter, and spacebar manually
                 -->
              <div v-if="questions[current].answer_type != 'Scale'" class="answer-option">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, 'yes')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, 'yes')"
                     id="yes" name="actions" value="yes" />
                <label for="yes">
                  <span class="answer-option-icon">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="53.333"
                      height="60.374"
                      viewBox="0 0 53.333 60.374"
                    >
                      <path
                        d="M.45,56.79V28.417c.756-.108,1.5-.217,2.251-.323,2.2-.316,4.406-.614,6.6-.956a6.914,6.914,0,0,0,2.9-1.34,21.977,21.977,0,0,0,4.31-4.183,54.38,54.38,0,0,1,3.966-4.166c1.019-1.011,2.14-1.92,3.166-2.924a12.953,12.953,0,0,0,3.512-6.419c.282-1.193.41-2.423.635-3.633.231-1.238.493-2.472.747-3.705.034-.161.057-.4.166-.457a2.029,2.029,0,0,1,.851-.293,6.2,6.2,0,0,1,6.289,3.9,9.9,9.9,0,0,1,.554,6.88,24.045,24.045,0,0,1-1.845,4.846c-.359.722-.834,1.389-1.178,2.117a5.6,5.6,0,0,0-.6,3.009,2.591,2.591,0,0,0,1.081,1.99,3,3,0,0,0,1.2.4,29.511,29.511,0,0,0,5.162.1c2.145-.055,4.287-.125,6.431-.151a14.653,14.653,0,0,1,3.635.369,5.137,5.137,0,0,1,2.079.956,3.688,3.688,0,0,1,1.308,3.206,8.048,8.048,0,0,1-.788,2.922c-.327.75-.7,1.48-1.06,2.234a.183.183,0,0,0,.038.055,4.4,4.4,0,0,1,1.518,4.34,7.218,7.218,0,0,1-2.319,4.268.319.319,0,0,0-.066.484,6.284,6.284,0,0,1-2.043,8.243c-.151.085-.3.168-.454.253a7.035,7.035,0,0,1-.265,4.33,7.112,7.112,0,0,1-3.38,3.646,14.854,14.854,0,0,1-4.688,1.516,32.121,32.121,0,0,1-10.9-.036,82.7,82.7,0,0,1-8.8-2.026,56.7,56.7,0,0,0-8.427-1.724,34.27,34.27,0,0,0-3.958-.253A22.1,22.1,0,0,0,5.3,56.21Q3,56.5.694,56.79A2.309,2.309,0,0,1,.45,56.79Z"
                        transform="translate(-0.35 -0.008)"
                        fill="#f7b239"
                      />
                      <path
                        d="M9.337,158.017c-9.395-4.24-8.157-16.048-9.1-24.377l-.248.037v29.106a1.8,1.8,0,0,0,.248,0c1.533-.2,3.063-.4,4.6-.595a22.187,22.187,0,0,1,2.771-.325,33.687,33.687,0,0,1,3.946.259,54.939,54.939,0,0,1,8.4,1.769,80.011,80.011,0,0,0,8.777,2.078,31.141,31.141,0,0,0,10.869.037,14.448,14.448,0,0,0,4.674-1.555,6.461,6.461,0,0,0,3.389-3.656C42.581,165.6,18.732,162.257,9.337,158.017Z"
                        transform="translate(0.01 -106.092)"
                        fill="#e09b2d"
                      />
                    </svg>
                  </span>
                  <span class="answer-option-text">Yes</span>
                </label>
              </div>
              <div v-if="questions[current].answer_type === 'Yes, No, Unsure'" class="answer-option">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, 'unsure')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, 'unsure')"
                     id="unsure" name="actions" value="unsure" />
                <label for="unsure">
                  <span class="answer-option-icon">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="58.308"
                      height="58.301"
                      viewBox="0 0 58.308 58.301"
                    >
                      <g transform="translate(0 0)">
                        <ellipse
                          cx="29.148"
                          cy="29.149"
                          rx="29.148"
                          ry="29.149"
                          transform="translate(0.011 0)"
                          fill="#f7b239"
                        />
                        <g transform="translate(0 6.596)">
                          <path
                            d="M559.45,50.9a28.86,28.86,0,0,1-2.456-38.01,28.461,28.461,0,0,0-2.787,2.456,28.848,28.848,0,0,0,40.8,40.8,29.363,29.363,0,0,0,2.456-2.787A28.86,28.86,0,0,1,559.45,50.9Z"
                            transform="translate(-545.755 -12.89)"
                            fill="#e09b2d"
                          />
                          <path
                            d="M592.14,62.941H581.178a1.2,1.2,0,0,1-1.2-1.006,1.183,1.183,0,0,1,1.166-1.356H592.1a1.2,1.2,0,0,1,1.2,1.006A1.183,1.183,0,0,1,592.14,62.941Z"
                            transform="translate(-558.015 -29.143)"
                            fill="#e09b2d"
                          />
                        </g>
                        <g transform="translate(10.735 21.064)">
                          <path
                            d="M598.106,54.63H568.128a1.2,1.2,0,0,0-1.2,1.006,1.183,1.183,0,0,0,1.166,1.356h29.978a1.2,1.2,0,0,0,1.2-1.006A1.183,1.183,0,0,0,598.106,54.63Z"
                            transform="translate(-564.834 -41.256)"
                            fill="#4d4d4d"
                          />
                          <path
                            d="M572.777,40.639A4.359,4.359,0,1,0,568.419,45,4.366,4.366,0,0,0,572.777,40.639Z"
                            transform="translate(-564.06 -36.28)"
                            fill="#4d4d4d"
                          />
                          <path
                            d="M607,36.28a4.359,4.359,0,1,0,4.359,4.359A4.366,4.366,0,0,0,607,36.28Z"
                            transform="translate(-574.521 -36.28)"
                            fill="#4d4d4d"
                          />
                        </g>
                      </g>
                    </svg>
                  </span>
                  <span class="answer-option-text">Unsure</span>
                </label>
              </div>
              <div v-if="questions[current].answer_type != 'Scale'" class="answer-option">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, 'no')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, 'no')"
                     id="no" name="actions" value="no" />
                <label for="no">
                  <span class="answer-option-icon">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="53.333"
                      height="60.485"
                      viewBox="0 0 53.333 60.485"
                    >
                      <path
                        d="M53.333,3.6V32.027c-.757.108-1.506.217-2.255.323-2.206.317-4.414.615-6.616.957a6.927,6.927,0,0,0-2.91,1.342,22.018,22.018,0,0,0-4.318,4.191,54.479,54.479,0,0,1-3.974,4.174c-1.021,1.013-2.144,1.923-3.172,2.929a12.977,12.977,0,0,0-3.519,6.431c-.283,1.2-.411,2.427-.636,3.64-.232,1.24-.494,2.476-.749,3.712-.034.162-.057.4-.166.457a2.033,2.033,0,0,1-.853.294,6.209,6.209,0,0,1-6.3-3.906,9.916,9.916,0,0,1-.555-6.892,24.088,24.088,0,0,1,1.849-4.854c.36-.723.836-1.391,1.181-2.121a5.61,5.61,0,0,0,.6-3.014A2.6,2.6,0,0,0,19.858,37.7a3,3,0,0,0-1.2-.4,29.568,29.568,0,0,0-5.171-.1c-2.149.055-4.3.126-6.444.151a14.681,14.681,0,0,1-3.642-.37,5.147,5.147,0,0,1-2.083-.957A3.694,3.694,0,0,1,0,32.8a8.062,8.062,0,0,1,.789-2.927c.328-.751.7-1.483,1.062-2.238a.184.184,0,0,0-.038-.055A4.412,4.412,0,0,1,.3,23.235a7.232,7.232,0,0,1,2.323-4.276.319.319,0,0,0,.066-.485,6.3,6.3,0,0,1,2.046-8.258c.151-.085.3-.168.455-.253a7.048,7.048,0,0,1,.266-4.338A7.126,7.126,0,0,1,8.839,1.972a14.882,14.882,0,0,1,4.7-1.519A32.183,32.183,0,0,1,24.458.49a82.862,82.862,0,0,1,8.82,2.029,56.813,56.813,0,0,0,8.443,1.727,34.335,34.335,0,0,0,3.965.253,22.142,22.142,0,0,0,2.785-.317q2.31-.287,4.618-.581A2.317,2.317,0,0,1,53.333,3.6Z"
                        transform="translate(0 -0.003)"
                        fill="#f7b239"
                      />
                      <g transform="translate(1.472 28.69)">
                        <path
                          d="M29.76,155.342a6.122,6.122,0,0,0-5.894-.412c-8.424,4.706-16.238,2.623-16.946,2.81a5.323,5.323,0,0,0,1.925.838,14.672,14.672,0,0,0,3.642.37c2.149-.026,4.3-.1,6.444-.151a29.776,29.776,0,0,1,5.171.1,3,3,0,0,1,1.2.4,2.6,2.6,0,0,1,1.083,1.993,5.59,5.59,0,0,1-.6,3.014c-.345.73-.821,1.4-1.181,2.121a25.508,25.508,0,0,0-1.583,3.9c1.3-2.41,4.308-6.926,4.308-6.926a33.689,33.689,0,0,1,3.248-4.235C31.877,157.914,31.438,156.4,29.76,155.342Z"
                          transform="translate(-6.92 -150.299)"
                          fill="#e09b2d"
                        />
                        <path
                          d="M195.247,136.382a21.637,21.637,0,0,0-7.2,6.173,17.981,17.981,0,0,1,1.549-1.242,6.925,6.925,0,0,1,2.91-1.342c2.2-.343,4.41-.64,6.616-.957.749-.109,1.5-.215,2.255-.323v-3.2A10.326,10.326,0,0,0,195.247,136.382Z"
                          transform="translate(-149.518 -135.357)"
                          fill="#e09b2d"
                        />
                      </g>
                    </svg>
                  </span>
                  <span class="answer-option-text">No</span>
                </label>
              </div>

              <div v-if="questions[current].answer_type === 'Scale'" class="answer-option no-text">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, '1')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, '1')"
                     id="1" name="actions" value="1" />
                <label for="1">
                  <span class="answer-option-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="56.399" height="56.399" viewBox="0 0 56.399 56.399"><g transform="translate(0)"><ellipse cx="28.198" cy="28.198" rx="28.198" ry="28.198" transform="translate(0.004 0)" fill="#f95428"/><g transform="translate(0 6.38)"><path d="M693.723,45.821A27.919,27.919,0,0,1,691.347,9.05a27.536,27.536,0,0,0-2.7,2.376,27.907,27.907,0,0,0,39.467,39.467,28.4,28.4,0,0,0,2.376-2.7A27.912,27.912,0,0,1,693.723,45.821Z" transform="translate(-680.475 -9.05)" fill="#e54728"/><path d="M717.365,54.379a1.118,1.118,0,0,1-.881-.423,1.139,1.139,0,0,1,.162-1.6,8.251,8.251,0,0,1,10.173-.141,1.136,1.136,0,1,1-1.382,1.8,5.976,5.976,0,0,0-7.353.106A1.136,1.136,0,0,1,717.365,54.379Z" transform="translate(-693.907 -24.164)" fill="#e54728"/></g><g transform="translate(10.004 17.164)"><path d="M707.052,35.67a4.135,4.135,0,0,0-.24-1.389,13.328,13.328,0,0,0,1.431.07,16.664,16.664,0,0,0,4.413-.62,1.136,1.136,0,1,0-.6-2.193c-8.079,2.2-11.576-2.94-11.724-3.165a1.144,1.144,0,0,0-1.572-.338,1.131,1.131,0,0,0-.338,1.572,10.141,10.141,0,0,0,2.369,2.383,4.213,4.213,0,1,0,6.26,3.68Z" transform="translate(-698.238 -27.855)" fill="#4d4d4d"/><path d="M719.555,43.75a13.2,13.2,0,0,0-12.492,8.728,1.139,1.139,0,1,0,2.15.754,11.025,11.025,0,0,1,20.684,0,1.142,1.142,0,0,0,1.072.761,1.125,1.125,0,0,0,.374-.063,1.146,1.146,0,0,0,.7-1.452A13.183,13.183,0,0,0,719.555,43.75Z" transform="translate(-700.822 -32.544)" fill="#4d4d4d"/><path d="M742.965,28.043a1.138,1.138,0,0,0-1.572.331c-.148.219-3.645,5.365-11.724,3.165a1.136,1.136,0,1,0-.6,2.193,16.664,16.664,0,0,0,4.413.62c.5,0,.973-.028,1.431-.07a4.138,4.138,0,0,0-.24,1.389,4.216,4.216,0,1,0,6.267-3.68,9.975,9.975,0,0,0,2.369-2.383A1.139,1.139,0,0,0,742.965,28.043Z" transform="translate(-707.087 -27.856)" fill="#4d4d4d"/></g></g></svg>
                  </span>
                  <span class="answer-option-text">Very angry</span>
                </label>
              </div>
              <div v-if="questions[current].answer_type === 'Scale'" class="answer-option no-text">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, '2')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, '2')"
                     id="2" name="actions" value="2" />
                <label for="2">
                  <span class="answer-option-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="56.18" height="56.173" viewBox="0 0 48.154 48.149"><g transform="translate(0)"><ellipse cx="24.073" cy="24.073" rx="24.073" ry="24.073" transform="translate(0.009 0)" fill="#f7b239"/><ellipse cx="24.073" cy="24.073" rx="24.073" ry="24.073" transform="translate(0.009)" fill="#fc5316" opacity="0.65"/><g transform="translate(0 5.448)" opacity="0.6" style="mix-blend-mode:multiply;isolation:isolate"><path d="M829.945,44.281a23.835,23.835,0,0,1-2.029-31.391,23.507,23.507,0,0,0-2.3,2.029,23.824,23.824,0,0,0,33.692,33.693,24.261,24.261,0,0,0,2.029-2.3A23.834,23.834,0,0,1,829.945,44.281Z" transform="translate(-818.635 -12.89)" fill="#e09b2d"/><path d="M862.665,65.106a.9.9,0,0,1-.349-.066,7.944,7.944,0,0,0-5.766,0,.97.97,0,0,1-.692-1.812,9.9,9.9,0,0,1,7.151,0,.971.971,0,0,1-.343,1.878Z" transform="translate(-835.661 -35.558)" fill="#e09b2d"/></g><g transform="translate(8.865 14.997)"><path d="M865.823,58.8a20.616,20.616,0,0,0-22.957,0,.972.972,0,1,0,1.083,1.613,18.678,18.678,0,0,1,20.791,0,.958.958,0,0,0,.542.169.971.971,0,0,0,.807-.427A.982.982,0,0,0,865.823,58.8Z" transform="translate(-839.127 -41.544)" fill="#4d4d4d"/><path d="M844.139,36.04a3.6,3.6,0,1,0-3.6,3.6A3.605,3.605,0,0,0,844.139,36.04Z" transform="translate(-836.94 -32.44)" fill="#4d4d4d"/><path d="M879.12,32.45a3.6,3.6,0,1,0,3.6,3.6A3.605,3.605,0,0,0,879.12,32.45Z" transform="translate(-852.298 -32.444)" fill="#4d4d4d"/></g></g></svg>
                  </span>
                  <span class="answer-option-text">Angry</span>
                </label>
              </div>
              <div v-if="questions[current].answer_type === 'Scale'" class="answer-option no-text">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, '3')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, '3')"
                     id="3" name="actions" value="3" />
                <label for="3">
                  <span class="answer-option-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="56.18" height="56.173" viewBox="0 0 56.18 56.173"><g transform="translate(0)"><ellipse cx="28.085" cy="28.085" rx="28.085" ry="28.085" transform="translate(0.011 0)" fill="#f7b239"/><ellipse cx="28.085" cy="28.085" rx="28.085" ry="28.085" transform="translate(0.011)" fill="#fc5316" opacity="0.4"/><g transform="translate(0 6.355)" opacity="0.4" style="mix-blend-mode:multiply;isolation:isolate"><path d="M558.95,49.513a27.807,27.807,0,0,1-2.367-36.623,27.419,27.419,0,0,0-2.686,2.367,27.8,27.8,0,0,0,39.308,39.308,28.282,28.282,0,0,0,2.367-2.686A27.807,27.807,0,0,1,558.95,49.513Z" transform="translate(-545.755 -12.89)" fill="#e09b2d"/><path d="M591.7,62.855H581.134a1.158,1.158,0,0,1-1.159-.969A1.14,1.14,0,0,1,581.1,60.58H591.66a1.158,1.158,0,0,1,1.159.969A1.14,1.14,0,0,1,591.7,62.855Z" transform="translate(-558.816 -30.291)" fill="#e09b2d"/></g><g transform="translate(10.343 20.295)"><path d="M596.967,54.63H568.084a1.158,1.158,0,0,0-1.159.969,1.14,1.14,0,0,0,1.124,1.306h28.883a1.158,1.158,0,0,0,1.159-.969A1.14,1.14,0,0,0,596.967,54.63Z" transform="translate(-564.91 -41.744)" fill="#4d4d4d"/><path d="M572.459,40.479a4.2,4.2,0,1,0-4.2,4.2A4.206,4.206,0,0,0,572.459,40.479Z" transform="translate(-564.06 -36.28)" fill="#4d4d4d"/><path d="M606.839,36.28a4.2,4.2,0,1,0,4.2,4.2A4.206,4.206,0,0,0,606.839,36.28Z" transform="translate(-575.547 -36.28)" fill="#4d4d4d"/></g></g></svg>
                  </span>
                  <span class="answer-option-text">Neutral</span>
                </label>
              </div>
              <div v-if="questions[current].answer_type === 'Scale'" class="answer-option no-text">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, '4')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, '4')"
                     id="4" name="actions" value="4" />
                <label for="4">
                  <span class="answer-option-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="56.18" height="56.173" viewBox="0 0 48.154 48.154"><g transform="translate(0 0)"><circle cx="24.077" cy="24.077" r="24.077" fill="#f7b239"/><circle cx="24.077" cy="24.077" r="24.077" fill="#fc5316" opacity="0.15"/><g transform="translate(0.003 4.99)" opacity="0.4" style="mix-blend-mode:multiply;isolation:isolate"><path d="M11.431,40.019A24.078,24.078,0,0,1,9.385,8.29a24.675,24.675,0,0,0-2.329,2.046A24.083,24.083,0,0,0,41.114,44.394a23.844,23.844,0,0,0,2.046-2.329A24.078,24.078,0,0,1,11.431,40.019Z" transform="translate(-0.005 -8.29)" fill="#e09b2d"/><path d="M36.658,63.206a15.8,15.8,0,0,1-4.268-.59.888.888,0,1,1,.482-1.709,14.065,14.065,0,0,0,7.572,0,.888.888,0,1,1,.482,1.709A15.6,15.6,0,0,1,36.658,63.206Z" transform="translate(-12.584 -29.134)" fill="#e09b2d"/></g></g><g transform="translate(9.858 16.156)"><path d="M30.92,54.359a18.783,18.783,0,0,1-13.333-5.514.89.89,0,0,1,1.258-1.258,17.1,17.1,0,0,0,24.149,0,.89.89,0,0,1,1.258,1.258A18.793,18.793,0,0,1,30.92,54.359Z" transform="translate(-16.7 -34.919)" fill="#4d4d4d"/><g transform="translate(0)"><path d="M22.875,30.063a3.293,3.293,0,1,0-3.293,3.293A3.293,3.293,0,0,0,22.875,30.063Z" transform="translate(-16.29 -26.77)" fill="#4d4d4d"/><path d="M54.913,26.77a3.293,3.293,0,1,0,3.293,3.293A3.293,3.293,0,0,0,54.913,26.77Z" transform="translate(-30.354 -26.77)" fill="#4d4d4d"/></g></g></svg>
                  </span>
                  <span class="answer-option-text">Happy</span>
                </label>
              </div>
              <div v-if="questions[current].answer_type === 'Scale'" class="answer-option no-text">
                <input type="radio" 
                    v-on:click="nextStep(questions[current].question_text, headlines[0].headline_text, '5')"
                    v-on:keyup.enter="nextStep(questions[current].question_text, headlines[0].headline_text, '5')"
                     id="5" name="actions" value="5" />
                <label for="5">
                  <span class="answer-option-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" width="56.18" height="56.173" viewBox="0 0 48.154 48.149"><g transform="translate(0)"><ellipse cx="24.073" cy="24.073" rx="24.073" ry="24.073" transform="translate(0.009 0)" fill="#f7b239"/><path d="M420.625,44.281A23.835,23.835,0,0,1,418.6,12.89a23.511,23.511,0,0,0-2.3,2.029,23.824,23.824,0,0,0,33.693,33.693,24.244,24.244,0,0,0,2.029-2.3A23.834,23.834,0,0,1,420.625,44.281Z" transform="translate(-409.315 -7.443)" fill="#e09b2d"/><path d="M457.2,40.23v4.539a17.605,17.605,0,0,1-24.9,0V40.23A17.594,17.594,0,0,0,457.2,40.23Z" transform="translate(-420.626 -20.532)" fill="#fff"/><path d="M450.871,61.68a5.312,5.312,0,0,1,4.677,2.793,12.468,12.468,0,0,1-15.7,0,5.318,5.318,0,0,1,7.849-1.746A5.292,5.292,0,0,1,450.871,61.68Z" transform="translate(-423.623 -30.063)" fill="#f95428"/><path d="M457.2,47.78v1.481a12.385,12.385,0,0,1-3.648,8.8,11.921,11.921,0,0,1-.963.867,5.308,5.308,0,0,0-7.837-1.74,5.308,5.308,0,0,0-7.837,1.74,12.425,12.425,0,0,1-4.611-9.667V47.78A17.605,17.605,0,0,0,457.2,47.78Z" transform="translate(-420.626 -24.089)" fill="#a81004"/><g transform="translate(6.913 11.271)"><path d="M431.165,30.688a.975.975,0,0,0,1.95,0,6.158,6.158,0,0,0-12.315,0,.975.975,0,1,0,1.95,0,4.207,4.207,0,1,1,8.415,0Z" transform="translate(-420.8 -24.53)" fill="#4d4d4d"/><path d="M470.7,24.53a6.165,6.165,0,0,0-6.158,6.158.975.975,0,0,0,1.95,0,4.213,4.213,0,0,1,8.427,0,.975.975,0,0,0,1.95,0A6.18,6.18,0,0,0,470.7,24.53Z" transform="translate(-442.518 -24.53)" fill="#4d4d4d"/></g></g></svg>
                  </span>
                  <span class="answer-option-text">Very happy</span>
                </label>
              </div>
            </div>
          </div>
        </fieldset>
      </div>
    </div>
    <div v-else-if="step === 2">
        <div class="main-content">
        <h1 class="title">Thanks for your responses!</h1>
        </div>
        <div class="main-actions">
        <div class="main-actions-group">
            <!-- Restarting from begining for prototype, but in actuality this would go to the next frame -->
          <button class="button" type="button" v-on:click="startQuiz(0)">
              <span class="icon-animated">
                  <svg class="icon-face" xmlns="http://www.w3.org/2000/svg" width="21.4" height="14.936" viewBox="0 0 21.4 14.936"><g transform="translate(0)"><path d="M27.771,52.729a14.432,14.432,0,0,1-10.244-4.236.684.684,0,0,1,.967-.967,13.137,13.137,0,0,0,18.556,0,.684.684,0,0,1,.967.967A14.44,14.44,0,0,1,27.771,52.729Z" transform="translate(-16.845 -37.793)" fill="#4d4d4d"/><path d="M21.35,29.3a2.53,2.53,0,1,0-2.53,2.53A2.53,2.53,0,0,0,21.35,29.3Z" transform="translate(-16.29 -26.77)" fill="#4d4d4d"/><path d="M54.15,26.77a2.53,2.53,0,1,0,2.53,2.53A2.53,2.53,0,0,0,54.15,26.77Z" transform="translate(-35.28 -26.77)" fill="#4d4d4d"/></g></svg>
                  <svg class="icon-head" xmlns="http://www.w3.org/2000/svg" width="37" height="37" viewBox="0 0 37 37"><circle cx="18.5" cy="18.5" r="18.5" fill="#f7b239"/><g transform="translate(0.002 3.834)" opacity="0.8"><path d="M8.784,32.669A18.5,18.5,0,0,1,7.213,8.29a18.959,18.959,0,0,0-1.79,1.572A18.5,18.5,0,0,0,31.592,36.031a18.321,18.321,0,0,0,1.572-1.79A18.5,18.5,0,0,1,8.784,32.669Z" transform="translate(-0.005 -8.29)" fill="#e09b2d"/><path d="M35.519,62.665a12.143,12.143,0,0,1-3.279-.453.682.682,0,1,1,.37-1.313,10.807,10.807,0,0,0,5.818,0,.682.682,0,0,1,.37,1.313A11.987,11.987,0,0,1,35.519,62.665Z" transform="translate(-17.021 -36.485)" fill="#e09b2d"/></g></svg>
              </span>
              View more headlines
            </button>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import Vue from "vue";
import Velocity from 'velocity-animate'

export default {
  name: "Home",
  data() {
    return {
      documentId: "",
      step: 0, // questions
      loading: true,
      settings: {
        headline_design: null
      },
      fields: {
        title: null,
        description: null
      },
      frames: [], // question groups (headline q vs)
      frameType: null,
      questions: [],
      headlines: [],
      answers: [],
      current: 0,
      currentTheme: 0,
      themes: [
          't-blue',
          't-green',
          't-red',
          't-yellow'
      ]
    };
  },
  methods: {
    // Load up app settings
    // Check for intro state
    getInitContent() {
      this.$prismic.client.getSingle("application").then(document => {
        if (document) {
          this.settings.headline_design = `is-${document.data.headline_design}`;

          this.fields.title = document.data.intro_title;
          this.fields.description = document.data.intro_text;

          // Get all the frame groups from the CMS (sets of question/headline pairs)
          this.frames = document.data.quiz_frames;

          // If there is a title present in the App content, show the intro step,
          // else go to the next step - ignoring description as it shouldn't exist without heading
          this.step = this.$prismic.richTextAsPlain(this.fields.title) ? 0 : 1;
          if (this.step === 1) {
            this.startQuiz();
          }

          this.loading = false;
        } else {
          this.$router.push({ name: "not-found" });
        }
      });
    },
    // Set up/reset quiz content
    startQuiz(frame) {
      this.step = 1;

      // reset everything on start.. in an actual app this wouldnt totally clear everything out, would move on to the next grameframe of questions
      this.current = 0;
      this.questions = [];
      this.headlines = [];
      this.currentTheme = 1;

      this.frameType = this.frames[0].options.type;

      this.$prismic.client
        .getByID(this.frames[0].options.id)
        .then(document => {
          // if it's a question group, there is only one question, find it
          // multiple headlines, find them
          if (this.frameType === "question_group") {
            this.getQuestionsById(document.data.question.id);
            document.data.headlines.forEach(h =>
              this.getHeadlinesById(h.headline.id)
            );

            // if it's a headline group, there is only one headline, find it
            // multiple questions, find them
          } else if (this.frameType === "headline_group") {
            this.getHeadlinesById(document.data.headline.id);
            document.data.questions1.forEach(q =>
              this.getQuestionsById(q.question_about_headline.id)
            );
          }
        });

      // Handle focus for keyboard once content is in place
      this.$nextTick(function() {
        this.$refs.main.focus();
      });
    },
    nextStep(q, h, a) {
        this.answers.push({
            question: q,
            headline: h,
            answer: a
        });
        // this.current++;
        if (this.frameType === "question_group" && this.current === this.questions.length ||
            this.frameType === "headline_group" && this.current === this.headlines.length) {
                this.step = 2;
                this.currentTheme = 0;

                // output final answers
                console.log(this.answers);
        } else {
            this.current++;
            this.currentTheme = this.currentTheme === this.themes.length - 1 ? 0 : this.currentTheme+=1;
        }
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