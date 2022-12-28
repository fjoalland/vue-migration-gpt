e
<template>
  <v-row no-gutters>
    <v-col>
      <codemirror
        :placeholder="code"
        :autofocus="true"
        :indent-with-tab="true"
        :extensions="extensions"
        @ready="handleReady"
        @change="log('change', $event)"
        @focus="log('focus', $event)"
        @blur="log('blur', $event)"
      />
    </v-col>

    <v-col>
      <codemirror
        :placeholder="code"
        disabled="true"
        :autofocus="true"
        :indent-with-tab="true"
        :extensions="extensions"
        @ready="handleReady"
        @change="log('change', $event)"
        @focus="log('focus', $event)"
        @blur="log('blur', $event)"
      />
    </v-col>
  </v-row>
</template>

<script>
import {ref, shallowRef} from 'vue'
import {Codemirror} from 'vue-codemirror'
import {javascript} from '@codemirror/lang-javascript'
import {oneDark} from '@codemirror/theme-one-dark'
import optionApiExample from 'raw-loader!../static/examples/option-api.txt'

export default {
  name: 'MigrationPage',
  components: {
    Codemirror
  },
  setup() {
    const code = ref(optionApiExample)
    const extensions = [javascript(), oneDark]
    const extensionsResult = [javascript()]

    // Codemirror EditorView instance ref
    const view = shallowRef()
    const handleReady = (payload) => {
      view.value = payload.view
    }

    // Status is available at all times via Codemirror EditorView
    const getCodemirrorStates = () => {
      const state = view.value.state
      const ranges = state.selection.ranges
      const selected = ranges.reduce((r, range) => r + range.to - range.from, 0)
      const cursor = ranges[0].anchor
      const length = state.doc.length
      const lines = state.doc.lines
      // more state info ...
      // return ...
    }

    return {
      code,
      extensions,
      extensionsResult,
      handleReady,
      log: console.log
    };
  }
}
</script>
