<template>
  <q-layout>
    <div slot="header" class="toolbar">
      <q-toolbar-title :padding="0">
      <button class="big capitalize" @click="redirect('dashboard')" v-if="loggedIn">Dashboard</button>
      <button class="big capitalize" @click="redirect('home')" v-else>Apiko + Quasar App</button>
      </q-toolbar-title>
      <button @click="$refs.leftDrawer.open()" v-if="loggedIn">
        <i>menu</i>
      </button>
    </div>
    <q-drawer ref="leftDrawer" :swipe-only="true" right-side>
      <userAvatar></userAvatar>
      <div class="list no-border platform-delimiter">
        <q-drawer-link icon="settings" :to="{path:'account-settings' , exact: true}">
        Account Settings
        </q-drawer-link>
        <div class="list no-border">
          <div class="item item-link" @click="logUserOut" v-go-back=" '/'">
          <i class="item-primary">undo</i>
            <div class="item-content">
              Logout
            </div>
          </div>
        </div>
      </div>
    </q-drawer>

    <!--
      Replace following "div" with
      <router-view class="layout-view"> component
      if using subRoutes
    -->
    <transition name='route' mode="out-in">
    <router-view class="layout-view"></router-view>
    </transition>
  </q-layout>
</template>

<script>
import { mapGetters, mapMutations } from 'vuex'
import userAvatar from './userAvatar.vue'

export default {
  components: {
    userAvatar
  },
  data () {
    return {
    }
  },
  computed: {
    ...mapGetters(['loggedIn'])
  },
  methods: {
    ...mapMutations(['logout']),
    redirect (address) {
      this.$router.push({name: address})
    },
    logUserOut () {
      this.$router.replace({path: '/'})
      this.$refs.leftDrawer.close()
      this.logout()
    }
  }
}
</script>

<style>
body {
  background-color: #f5f8fa;
}
.route-enter {
  transform: translateX(600px);
  opacity: 0;
}
.route-enter-active {
  transition: all 0.5s ease;
}
.route-leave {
  opacity: 0;
}
</style>
