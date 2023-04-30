<template>
  <div class="is-editor-wrapper">
        <div class="buttonsWrapper">
            <transition name="fade" mode="out-in">
                <span :key="1">
                  <label class="label is-pointer noselect button is-primary">
                    <input @change="fileInput" accept=".txt, .c" type="file" required/>
                    <span>
                      <i class="fas fa-upload pl-1 pr-2" /> Import
                    </span>
                  </label>
                  <button
                      v-if="(!iOS || shareAvailable)"
                      :key="1"
                      class="button is-primary ml-4 pl-5 pr-4 mt-4 is-save-button"
                      @click="saveFileModal"
                  >
                    Save
                    <i class="fas fa-download pl-1 pr-2" />
                  </button>
                </span>
            </transition>
        </div>
        <br>
        <br>
        <transition name="fade" mode="out-in">
            <div :key="1">
                <prism-editor
                    class="my-editor"
                    v-model="code"
                    :highlight="highlighter"
                    line-numbers />
            </div>
        </transition>

        <div class="is-bottom-nav" />

<!--        <button
            v-if="iosLiteApp"
            @click="webviewTrigger"
            class="button is-ads-button is-border-secondary mt-5"
        >
          Get Rid of ads
        </button>-->

        <SaveModal
            v-if="saveFileModalOpen"
            @save="downloadFile"
            @close="saveFileModalOpen=false"
        />
  </div>
</template>

<script>
import SaveModal from '@/components/modals/SaveModal'
import { PrismEditor } from 'vue-prism-editor'
import 'vue-prism-editor/dist/prismeditor.min.css'
import { highlight, languages } from "prismjs/components/prism-core"
import Prism from "prismjs"
import 'prismjs/components/prism-clike'
import 'prismjs/components/prism-c'
import 'prismjs/themes/prism-tomorrow.css'

export default {
    name: 'Editor',
    components: {
        SaveModal,
        PrismEditor
    },
    props: {
        share: {
            type: Boolean,
            required: true
        }
    },
    data () {
        return  {
            inputText: '',
            saveFileModalOpen: false,
            shareAvailable: false,
            code: '\n\n\n\n\n\n\n\n\n\n\n\n\n\n',
            c: {}
        }
    },
    watch: {
        inputFile (val) {
            if (val != '') {
                this.code = val
                this.$store.state.inputFile = ''
            }
        },
        share (val) {
            if (val) this.shareFile()
        }
    },
    computed: {
        iOS () {
            return this.$store.state.iOS
        },
        inputFile () {
            return this.$store.state.inputFile
        },
        iosLiteApp () {
          return window.webkit && window.webkit.messageHandlers
        }
    },
    created () {
        this.shareAvailable = window.navigator.share
    },
    methods: {
        saveFileModal () {
            if (!this.iOS) {
                this.saveFileModalOpen = true
                return
            }
            this.shareFile()
        },
        createFileLink (fileName) {
            const file = new Blob([this.code], { type: "data:text/csv;charset=utf-8" }, `${fileName}.c`)
            return window.URL.createObjectURL(file)
        },
        downloadFile (fileName) {
            const link = this.createFileLink (fileName)

            let hiddenElement = document.createElement('a')
            hiddenElement.href = link
            hiddenElement.download = `${fileName}.c`
            hiddenElement.click();
            this.saveFileModalOpen = false
        },
        shareFile () {
            if (this.code.length) {
                navigator.share({
                    "title": 'js File',
                    "text": this.code
                })
            }
        },
        fileInput (event) {
          const file = event.target.files[0];
          const reader = new FileReader();
          reader.onload = e => this.code =  e.target.result
          reader.readAsText(file);
          this.settings = false
        },
        highlighter(code) {
          if (Prism.languages.c) {
            this.c = Prism.languages.c
          }
          return highlight(code, this.c, '')
        },
    }
}
</script>
