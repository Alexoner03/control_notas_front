<template>
  <q-layout view="lHr LpR lFf">

    <q-header reveal class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="drawer = !drawer" />

        <q-toolbar-title>
          <q-avatar size="4rem">
            <img src="../assets/icons/logo-white.svg">
          </q-avatar> &nbsp;
          CONTROL DE NOTAS
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="drawer"
      :width="200"
      elevated
      :breakpoint="400"
    >
      <q-scroll-area style="height: calc(100% - 150px); margin-top: 150px; border-right: 1px solid #ddd">
        <q-list padding>
          <q-item to="/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="home" color="secondary"/>
            </q-item-section>

            <q-item-section>
              INICIO
            </q-item-section>
          </q-item>

          <q-item to="/users" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="account_box" color="secondary"/>
            </q-item-section>

            <q-item-section>
              USUARIOS
            </q-item-section>
          </q-item>

          <q-item to="/courses" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="storefront" color="secondary"/>
            </q-item-section>

            <q-item-section>
              CURSOS
            </q-item-section>
          </q-item>

          <q-item @click="logout" clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="close" color="secondary" />
            </q-item-section>

            <q-item-section>
              CERRAR SESIÓN
            </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img class="absolute-top" src="../assets/img/wallImage.jpg" style="height: 150px" img-class="filter">
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="../assets/img/pedrito.jpg">
          </q-avatar>
          <div class="text-weight-bold">{{ nombres ? nombres : 'Usuario' }}</div>
          <div>@administrador</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <transition
        enter-active-class="animated fadeInLeft"
        leave-active-class="animated fadeOutRight"
        appear
        mode="in-out"
        :duration="300"
      >
        <router-view />
      </transition>
    </q-page-container>

  </q-layout>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import {QSpinnerCube} from "quasar";
export default {
  name :'mainLayout',
  data () {
    return {
      left: false,
      drawer: false,
    }
  },
  methods : {
    async logout(){
      const dialog = this.$q.dialog({
        title: 'Cerrando Sesión',
        message: 'Espere por favor',
        progress: {
          spinner: QSpinnerCube,
          color: 'primary'
        },
        persistent: true, // we want the user to not be able to close it
        ok: false // we want the user to not be able to close it
      })
      await this.logoutUser()
      dialog.hide()
      await this.$router.replace('/login')
    },
    ...mapActions('auth',['logoutUser'])
  },
  computed : {
    ...mapState('auth',['nombres'])
  }
}
</script>
<style>
.filter{
  filter: blur(0.1rem) brightness(0.5);
}
</style>
