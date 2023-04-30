<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        iOS: false
      }
    },
    created () {
      if (this.iosLiteApp) {
        setTimeout(() => {
          //this.showInterstitial()
        }, 20000)
        setInterval(() => {
          //this.showInterstitial()
        }, 70000)
      }

      this.iOS = [
      'iPad Simulator',
      'iPhone Simulator',
      'iPod Simulator',
      'iPad',
      'iPhone',
      'iPod'
    ].includes(navigator.platform) || (navigator.userAgent.includes("Mac") && "ontouchend" in document)
    this.$store.state.iOS = this.iOS
    },
    computed: {
      iosLiteApp () {
        return window.webkit && window.webkit.messageHandlers
      }
    },
    methods: {
      showInterstitial () {
        if (this.iosLiteApp) {
          window.webkit.messageHandlers.showInterstitial.postMessage({
            "message": 'showInterstitial'
          })
        }
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
