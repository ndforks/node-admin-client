<template>
  <q-page padding>
    <div class="box row title">
      <div class="page layout-padding">
        <img src="~assets/alinex.png" alt="Alinex Logo" />
        <h1 class="center-text">{{ $t('title') }}</h1>
      </div>
    </div>
    <div class="icons row">
      <q-btn flat round wait-for-ripple
        @click="$router.push('/info')" icon="info" >
        <q-tooltip anchor="bottom middle" self="top middle" :delay="500" class="q-mt-sm">
          <b>{{ $t('core.info.title')}}</b><br />
          {{$t('core.info.subtitle')}}
        </q-tooltip>
      </q-btn>
      <q-btn flat round wait-for-ripple :class="notAuthenticatedClass() + ' warning'"
        @click="login()" icon="vpn key">
        <q-tooltip anchor="bottom middle" self="top middle" :delay="500" class="q-mt-sm">
          <b>{{ $t('layout.menu.login') }}</b><br />
          {{ $t('layout.menu.loginSub') }}
        </q-tooltip>
      </q-btn>
      <q-btn flat round wait-for-ripple :class="authenticatedClass()"
        @click="$router.push('/users')" icon="account box" >
        <q-tooltip anchor="bottom middle" self="top middle" :delay="500" class="q-mt-sm">
          <b>{{ $t('core.users.title')}}</b><br />
          {{$t('core.users.subtitle')}}
        </q-tooltip>
      </q-btn>
      <q-btn flat round wait-for-ripple :class="authenticatedClass()"
        @click="$router.push('/chat')" icon="chat" >
        <q-tooltip anchor="bottom middle" self="top middle" :delay="500" class="q-mt-sm">
          <b>{{ $t('colab.chat.title')}}</b><br />
          {{$t('colab.chat.subtitle')}}
        </q-tooltip>
      </q-btn>
    </div>
  </q-page>
</template>

<style lang="stylus" scoped>
  main
    background: url('~assets/background.jpg') no-repeat center center fixed
    background-size: cover

  .box
    display: table
    margin: 0 auto
    border: 3px solid #aaa
    border-radius: 40px
    background: #eee
    opacity: 0.8
    text-align: center
    &.title
      margin-bottom: 40px
      max-width: 90%

  .icons
    justify-content: center
    button
      background: #eee
      opacity: 0.6
      margin: 10px
      font-size: 32px
      height: 2.5em
      width: 2.5em
      &.warning
        color: white
        background: #f0ad4e
        opacity: 0.8

  .title img
    max-width: 90%

  h1
    margin: 0
    color: #f0ad4e
    font-size: 26px
    font-weight: 900

  @media (min-width: 420px)
    .title img
      max-width: 90%
    h1
      font-size: 32px
    .icons button
      font-size: 48px

  @media (min-width: 767px)
    .title img
      max-width: 100%
    h1
      font-size: 36px
</style>

<script>
import { AuthMixin } from '../mixins/auth'

export default {
  name: 'PageIndex',
  methods: {
    login () {
      this.$store.commit('layout/login', true)
    }
  },
  mixins: [AuthMixin]
}
</script>
