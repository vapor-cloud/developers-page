<template>
  <div class="flex justify-center">
    <section class="vapor-terminal">
      <div class="vapor-terminal__stoplight-container">
              <div class="vapor-terminal__stoplight"
                   v-for="color in lights"
                   :class="color"></div>
          </div>

          <pre class="vapor-terminal__sample-code" v-html="sampleCode"></pre>
    </section>
  </div>
</template>
<style lang="scss">
    @import 'src/variables';
    @import 'src/mixins';

    .vapor-terminal {
        background: black;
        width: 45em;
        max-width: 85%;
        padding: 1em;
        border-radius: 1em;

        .vapor-terminal__stoplight-container {
            .vapor-terminal__stoplight {
                display: inline-block;
                width: 1.5em;
                height: 1.5em;
                border-radius: 1em;

                &.red {
                    background: $color-red;
                }
                &.yellow {
                    background: $color-yellow;
                }
                &.green {
                    background: $color-green;
                }
            }

            .vapor-terminal__stoplight + .vapor-terminal__stoplight {
                margin-left: .75em;
            }
        }

        .vapor-terminal__sample-code {
            color: white;
            font-size: large;
            font-weight: 500;
            font-family: 'Roboto Mono', monospace;
            padding: 0 .75em;

            & .keyword {
                color: $color-code-keyword;
            }

            & .type {
                color: $color-code-type;
            }

            & .method {
                color: $color-code-method;
            }

            & .string {
                color: $color-code-string;
            }
        }
    }

    @include respond-to(phone) {
        .vapor-terminal {
            max-width: 90%;

            .vapor-terminal__stoplight {
                width: 1em !important;
                height: 1em !important;
            }

            .vapor-terminal__stoplight + .vapor-terminal__stoplight {
                margin-left: .5em !important;
            }

            .vapor-terminal__sample-code {
                font-size: x-small !important;
                padding: 0;
            }
        }
    }

    @include respond-to(phablet) {
        .vapor-terminal {
            .vapor-terminal__sample-code {
                font-size: small;
            }
        }
    }

    @include respond-to(tablet) {
        .vapor-terminal {
            .vapor-terminal__stoplight {
                width: 1.25em !important;
                height: 1.25em !important;
            }

            .vapor-terminal__stoplight + .vapor-terminal__stoplight {
                margin-left: .6em !important;
            }

            .vapor-terminal__sample-code {
                margin: 0;
            }
        }
    }
</style>

<script>
    class KEYWORDS {
        static get IMPORT() {
            return this._generateHTML('import');
        }

        static get LET() {
            return this._generateHTML('let');
        }

        static get TRY() {
            return this._generateHTML('try');
        }

        static get SELF() {
            return this._generateHTML('self');
        }

        static get IN() {
            return this._generateHTML('in');
        }

        static get RETURN() {
            return this._generateHTML('return');
        }

        static _generateHTML(text) {
            return `<span class="keyword">${text}</span>`;
        }
    }

    export default {
        name: 'index',
        data () {
            return {
                lights: ['red', 'yellow', 'green']
            }
        },
        computed: {
            sampleCode() {
                return `
~ $ curl --request POST \\
  --url https://api-new.v2.vapor.cloud/v1/application \\
  --header 'authorization: Bearer MY_TOKEN' \\
  --header 'content-type: application/json' \\
  --data '{
	"project": {
		"id": "PROJECT_ID"
	},
	"repoName": "my-application",
	"name": "My Application",
	"environment": {
		"name": "production"
	}
}'
                `;
            }
        },
        methods: {
            method(text) {
                return this._generateHTML('method', text);
            },
            type(text) {
                return this._generateHTML('type', text);
            },
            string(text) {
                return this._generateHTML('string', text);
            },
            _generateHTML(className, text) {
                return `<span class="${className}">${text}</span>`;
            }
        }
    }
</script>
