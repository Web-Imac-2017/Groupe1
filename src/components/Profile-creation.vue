<template lang="html">
  <md-layout tag="form" novalidate @submit.stop.prevent="addProfile">
    <md-input-container>
      <label>Image de profil</label>
      <md-file v-model="pict" accept="image/*"></md-file>
    </md-input-container>
    <md-input-container id="profile-creation-name">
      <label>Nom du profil</label>
      <md-input v-model="profile.name" required></md-input>
      <span class="md-error">Ce nom de profil est déjà utilisé.</span>
    </md-input-container>
    <md-input-container id="profile-creation-desc">
      <label>Description</label>
      <md-textarea v-model="profile.desc" required></md-textarea>
      <span class="md-error">Votre message comporte des caractères non autorisés.</span>
    </md-input-container>
    <md-switch v-model="profile.isPrivate" id="privateSwitch1" name="privateSwitch" class="md-primary">Visible des autres utilisateurs.</md-switch>
    <md-button type="reset" @click.native="cancel">
      Annuler
    </md-button>
    <md-button type="submit" @click.native="addProfile" class="md-raised md-primary">
      Ajouter ce profil
    </md-button>
  </md-layout>
</template>

<script>
import Vuex from 'vuex'
import store from './connectionStore.js'
import formVerifications from './../formVerifications.js'
import apiRoot from './../config.js'

export default {
  name: 'profile-creation',
  store: store,
  data () {
    return {
      profile: {
        pict: null,
        name: '',
        desc: '',
        isPrivate: true
      }
    }
  },
  mixins: [formVerifications],
  methods: {
    ...Vuex.mapActions({
      addProfileStore: 'addProfile'
    }),
    cancel () {
      this.profile.pict = null
      this.profile.name = ''
      this.profile.desc = ''
      this.profile.isPrivate = false
      this.step = 'Annuler'
      this.$emit('close')
    },
    addProfile () {
      if (!(this.verif_name(this.profile.name, 'profile-creation-name') &&
            this.verif_text(this.profile.desc, 'profile-creation-desc'))) return
      var ban = false
      this.banned_word(this.profile.name).catch(() => {
          this.profile.name = 'Mot Banni : ' + this.profile.name
          ban = true
      })
      if(ban) return
      var priv = 0
      if (this.profile.isPrivate) priv = 1;

      this.$http.post(apiRoot + 'profile/create/', {
          profileName: this.profile.name,
          profileDesc: this.profile.desc,
          profilePrivate: this.profile.isPrivate
        }).then((response) => {
        console.log('SUCCESS: profile creation', response)
        this.addProfileStore(response.data.data.profileID)
        this.addAvatar(response.data.data.profileID)
        this.$emit('close')
      }, (response) => {
        console.error('ERR: profile creation request', response)
        switch (response.status) {
          case 400:
            console.log('Bad request')
            break
          case 409:
            console.log('Conflict')
            document.getElementById('profile-creation-name').classList.add('md-input-invalid')
            break
          default:
            console.log('Unknown error')
        }
      })
    },
    addAvatar (profileID) {
      if (this.profile.pict === null || !(this.profile.pict.type.match('image.*'))) return

      var profilePicture = new FormData()
      profilePicture.append('profilePicture', profilePicture, profilePicture.name)
      this.$http.get(apiRoot + 'profile/setPicture/' + profileID, profilePicture).then((response) => {
        console.log('SUCCESS: profile avatar modification', response)
      }, (response) => {
        console.error('ERR: profile avatar modification', response)
        switch (response.status) {
          case 400:
            console.log('Bad request')
            break
          case 401:
            console.log('Not authorized')
            break
          case 404:
            console.log('Not found')
            break
          case 406:
            console.log('Not accepted')
            break
          default:
            console.log('Unknown error')
        }
      })
    }
  }
}
</script>

<style lang="css">
</style>
