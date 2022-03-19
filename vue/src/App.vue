<template>
  <div>
    <SpTheme>
      <SpNavbar
        :links="navbarLinks"
        :activeRoute="router.currentRoute.value.path"
      />
      <router-view />
    </SpTheme>
  </div>
</template>

<script lang="ts">
import { computed, onBeforeMount } from 'vue'
import { useStore } from 'vuex'
import { SpTheme, SpNavbar, SpTx } from '@starport/vue'
import { useRouter } from 'vue-router'

export default {
  components: { SpTheme, SpNavbar, SpTx },

  setup() {
    // store
    let $s = useStore()

    // router
    let router = useRouter()

    // state
    let navbarLinks = [
      { name: 'Portfolio', url: '/portfolio' },
      { name: 'Data', url: '/data' }
    ]

    // computed
    let address = computed(() => $s.getters['common/wallet/address'])

    // lh
    onBeforeMount(async () => {
      await $s.dispatch('common/env/init',{
      apiNode: 'http://188.166.189.139:1317',
      rpcNode: 'http://188.166.189.139:26657',
      wsNode: 'ws://188.166.189.139:26657/websocket',
      chainId: 'mars',
      addrPrefix: 'cosmos',
      sdkVersion: 'Stargate',
      getTXApi: 'http://188.166.189.139:26657/tx?hash=0x'
   })

      router.push('portfolio')
    })

    return {
      navbarLinks,
      // router
      router,
      // computed
      address
    }
  }
}
</script>

<style scoped lang="scss">
body {
  margin: 0;
}
</style>
