<template>
  <div>
    <topNav @fileInput="fileInput" @share="shareFile" />
    <Editor :share="share" />
<!--
    <AdCard v-if="!iOS" class="mt-5" />
-->
    <RateModal v-if="showRateModal" @openStore="openStore" @close="showRateModal=false" />
  </div>
</template>

<script>
import Editor from '@/components/Editor.vue'
import topNav from '@/components/TopNav.vue'
import AdCard from "@/components/ads/AdCard"
import RateModal from "@/components/modals/RateModal"

export default {
  name: 'Home',
  components: {
    Editor,
    topNav,
    AdCard,
    RateModal
  },
  data () {
    return {
      inputFile: '',
      share: false,
      showRateModal: false
    }
  },
  computed: {
    iOS () {
      return this.$store.state.iOS
    }
  },
  created() {
    /*if (!this.iOS) {
      setTimeout(() => {
        if (Math.floor(Math.random() * (5)) === 3) {
          this.showRateModal = true
        }
      }, 60 * 1000)
    }*/
  },
  methods: {
    fileInput (file) {
      this.$store.state.inputFile = file
    },
    shareFile () {
      this.share = true
      setTimeout(() => {
        this.share = false
      }, 300)
    },
    openStore () {
      window.location.href = 'https://play.google.com/store/apps/details?id=app.markdowneditor.jl.com'
    }
  }
}
</script>
