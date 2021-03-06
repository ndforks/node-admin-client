<template>
  <q-layout view="lHh Lpr lFf" ref="layout">
    <q-layout-header>
      <q-toolbar color="primary" glossy>
        <q-btn flat dense round
          @click="leftDrawerOpen = !leftDrawerOpen"
          :title="$t('layout.sidebarHide')"
          >
          <q-icon name="menu" />
        </q-btn>

        <q-toolbar-title>
          {{ $t('title') }}
          <div slot="subtitle">{{ subtitle }}</div>
        </q-toolbar-title>
        <q-btn-dropdown flat round dense icon="language" :title="$t('layout.menu.language')">
          <q-list link>
            <q-item v-close-overlay @click.native="lang = 'en-us'"
              :disabled="this.$i18n.locale === 'en'">
              English
            </q-item>
            <q-item v-close-overlay @click.native="lang = 'de'"
              :disabled="this.$i18n.locale === 'de'">
              Deutsch
            </q-item>
          </q-list>
        </q-btn-dropdown>
        <q-btn flat icon="vpn key"
          :class="notAuthenticatedClass()"
          :title="$t('layout.menu.login')"
          @click="$store.commit('layout/login')">
        </q-btn>
        <q-btn-dropdown flat round dense icon="account circle"
          :class="authenticatedClass()"
          :title="$t('layout.menu.settings')">
          <q-list link>
            <q-item to="/user" class="hidden">
              <q-item-side icon="account circle" />
              <q-item-main :label="$t('layout.menu.profile')" />
            </q-item>
            <q-item v-close-overlay @click.native="logout">
              <q-item-side icon="exit to app" />
              <q-item-main :label="$t('layout.menu.logout')" />
            </q-item>
          </q-list>
        </q-btn-dropdown>
        <q-btn flat icon="security"
          :title="$t('policy.title')"
          @click="$router.push('/info')">
        </q-btn>
      </q-toolbar>
    </q-layout-header>

    <q-layout-drawer v-model="leftDrawerOpen" content-class="bg-grey-2">
      <sidebar-menu />
    </q-layout-drawer>

    <q-page-container>
      <router-view />
      <login-dialog />
    </q-page-container>

  </q-layout>
</template>

<script>
import { AuthMixin } from '../mixins/auth'
import SidebarMenu from '../components/sidebarMenu'
import LoginDialog from '../components/loginDialog'

export default {
  name: 'LayoutDefault',
  data () {
    return {
      leftDrawerOpen: true,
      lang: this.$q.i18n.lang
    }
  },

  computed: {
    subtitle () {
      return this.$route.meta.module
        ? this.$i18n.t(`${this.$route.meta.module}.title`) + ': ' + this.$i18n.t(`${this.$route.meta.module}.subtitle`)
        : this.$route.meta.title
    }
  },
  mounted () {
    this.checkAuthentication()
    this.$store.commit('layout/login', this.$route.path === '/login')
  },
  watch: {
    '$route' () {
      this.checkAuthentication()
      this.$store.commit('layout/login', this.$route.path === '/login')
    },
    lang (lang) {
      // Update application specific i18n (only use major language part)
      this.$i18n.locale = lang.split('-')[0]
      // Update quasar components i18n
      // (dynamic import, so loading on demand only)
      import(`quasar-framework/i18n/${lang}`).then(lang => {
        this.$q.i18n.set(lang.default)
      })
    }
  },
  components: { SidebarMenu, LoginDialog },
  mixins: [AuthMixin]
}
</script>
