<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h1>vuetify-jsonschema-form-read-evaluate-print-loop</h1>
    </v-app-bar>

    <div class="grid">
      <div class="pa-6">
        <v-btn @click="reset">Reset</v-btn>
      </div>
      <div></div>
      <div>
        <v-textarea solo hide-details id="editor" height="100%" class="py-6 pl-6 pr-3" v-model="code"></v-textarea>
      </div>
      <div>
        <div class="py-6 pl-3 pr-6" style="height: 100%">
          <v-card v-if="error === ''" class="pa-4" height="100%" style="overflow-y: auto">
            <v-form v-model="valid">
              <v-jsf v-model="model" :schema="schema"/>
            </v-form>
          </v-card>
          <v-alert v-else type="warning">
            {{ error }}
          </v-alert>
        </div>
      </div>
    </div>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import VJsf from '@koumoul/vjsf/lib/VJsf.js'
import '@koumoul/vjsf/lib/VJsf.css'
import '@koumoul/vjsf/lib/deps/third-party.js'

import basic from './basic.json'

export default Vue.extend({
  name: 'App',

  components: {
    VJsf,
  },

  data: () => ({
    valid: false,
    model: {},
    code: "",
    error: "",
    schema: basic,
  }),

  watch: {
    code: function(val) {
      try {
        let s = JSON.parse(val);
        this.error = "";
        this.schema = s;
      } catch (e) {
        this.error = "Invalid JSON: " + e;
        this.schema = {};
      }
    }
  },
  methods: {
    reset() {
      this.code = JSON.stringify(basic, null, 2);
    }
  },

  mounted() {
    this.reset();
  }
});
</script>

<style>
html, body {
  overflow: hidden;
}

.grid {
  display: grid !important;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 64px 1fr;
  grid-column-gap: 0;
  grid-row-gap: 0;
  align-items: stretch;

  height: 100vh;
  width: 100vw;
  min-height: 0; /* NEW */
  min-width: 0; /* NEW; needed for Firefox */
  padding: 64px 0 0;

  background-color: #333;
}

.grid > div {
  height: 100%;

  overflow: hidden; /* NEW */
  min-height: 0; /* NEW; needed for Firefox */
}

.v-textarea, .v-input__control, .v-text-field__slot {
  height: 100%;
}

#editor {
  padding: 4px !important;
}

.v-input__slot:after {
  border: none !important;
}
</style>