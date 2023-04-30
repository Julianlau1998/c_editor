<template>
    <div>
        <div class="is-modal-wrapper"></div>
        <div class="modal is-small">
                <h2>
                    Rate this app
                </h2>
                <p style="margin-top: -1rem;">
                  If you like this app, please support us by leaving a rating in the Play Sotre.
                </p>
              <span class="">
                <button @click="close" class="button is-secondary pl-3 pr-3 mt-3">
                    Close
                </button>
                <button @click="rateApp" class="button is-primary pl-4 ">
                    Rate now
                    <i class="fas fa-star pl-1 pr-2" />
                </button>
              </span>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            fileName: ''
        }
    },
    methods: {
        onSubmit () {
            if (this.fileName.length) {
                this.$emit('save', this.fileName)
            }
        },
        close () {
            this.$emit('close')
        },
        rateApp () {
          this.$emit('openStore')
        }
    }
}
</script>
