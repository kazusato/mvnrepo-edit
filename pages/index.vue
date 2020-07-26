<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <div class="text-h3">
      MVN REPO EDITOR
    </div>

    <v-row class="col-12">
      <v-col class="col-6">
        <v-textarea id="original" outlined label="Original" class="caption" @input="editText" v-model="originalText"></v-textarea>
      </v-col>
      <v-col class="col-6">
        <v-textarea id="edited" outlined label="Edited" class="caption" v-model="editedText"></v-textarea>
      </v-col>
    </v-row>

    <v-row class="col-12">
      <v-col>
        <v-switch v-model="useSingleQuote" label="シングルクオート" @change="editText"></v-switch>
      </v-col>
      <v-col>
        <v-switch v-model="useParenthesis" label="括弧" @change="editText"></v-switch>
      </v-col>
    </v-row>

  </v-layout>
</template>

<script>
export default {
  data() {
    return {
      originalText: '',
      editedText: '',
      useSingleQuote: false,
      useParenthesis: false
    }
  },
  methods: {
    editText: function () {
      const lines = this.originalText.split(/\r\n|\n/)
      if (lines.length >= 2) {
        const chunks = lines[1].split(' ')
        let scope
        if (chunks[0] === 'compile') {
          scope = 'implementation'
        } else if (chunks[0] === 'testCompile') {
          scope = 'testImplementation'
        } else {
          this.editedText = 'unknown'
          return
        }
        const group = chunks[2].replace(/'/g, "").replace(",", "")
        const artifact = chunks[4].replace(/'/g, "").replace(",", "")
        const version = chunks[6].replace(/'/g, "").replace(",", "")

        // this.editedText = `${scope}("${group}:${artifact}:${version}")`
        let edited = scope
        edited += this.useParenthesis ? "(" : " "
        edited += this.useSingleQuote ? "'" : '"'
        edited += `${group}:${artifact}:${version}`
        edited += this.useSingleQuote ? "'" : '"'
        edited += this.useParenthesis ? ")" : ""
        this.editedText = edited
      } else {
        this.editedText = 'invalid text'
      }
    }
  }
}
</script>

<style>
</style>
