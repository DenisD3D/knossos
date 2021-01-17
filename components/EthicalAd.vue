<template>
  <div>
    <div
      v-if="!showAlt"
      :id="adId"
      class="ethical-ad"
      data-ea-publisher="modrinth-com"
      :data-ea-type="type"
      data-ea-manual="true"
    />
    <div v-else class="alt">
      <p>{{ $t('ethical_ad_alt') }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EthicalAd',
  props: {
    type: {
      type: String,
      default: 'text',
    },
    adId: {
      type: String,
      default: 'none',
    },
  },
  data() {
    return {
      showAlt: false,
    }
  },
  mounted() {
    try {
      // eslint-disable-next-line no-undef
      if (typeof ethicalads === 'undefined') {
        this.$notify({
          group: 'ads',
          title: this.$t('ethical_ad_notify_title'),
          text: this.$t('ethical_ad_notify_text'),
          type: 'error',
        })
        this.showAlt = true
      } else {
        const element = document.getElementsByClassName('ethical-ad')[0]
        element.className = 'ethical-ad'
        element.innerHTML = ''
        // eslint-disable-next-line no-undef
        ethicalads.load()
        element.className = 'ethical-ad loaded ' + this.$colorMode.preference
      }
    } catch (err) {
      // eslint-disable-next-line no-console
      console.error(err)
    }
  },
}
</script>

<style lang="scss" scoped>
[data-ea-type='text'] {
  min-height: 70px;
}
[data-ea-type='image'] {
  margin: auto 10px;
  min-height: 260px;
}
.alt {
  font-size: 14px;
  border-radius: var(--size-rounded-sm);
  background-color: var(--color-raised-bg);
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.15);
  padding: 0.7em 1em;
  margin: 1em 0 2em 0;
}
</style>
