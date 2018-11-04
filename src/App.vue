<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vuetify</span>
        <span class="font-weight-light"> FIREBASE CRUD</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
      >
        <span class="mr-2">Sign up</span>
      </v-btn>
    </v-toolbar>
        <v-container>
          <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <v-card></v-card>
            </v-flex>
          </v-layout>
        </v-container>
    <v-content>
      <v-container>
        <v-layout row>
          <v-flex xs12 sm6 offset-sm3>
            <v-card>
              <v-text-field label="Name" v-model="name"></v-text-field>
              <v-btn @click="submitName">Add</v-btn>
            </v-card>
          </v-flex>
        </v-layout>

      </v-container>
      <v-container>
        <v-layout row v-for="personName in names" :key="personName['.key']">
          <v-flex xs12 sm6 offset-sm3 v-if="! personName.edit">
            <v-list> 
              {{ personName.name }}
            </v-list>
            <v-btn @click="editName(personName['.key'])">Edit</v-btn>
            <v-btn @click="deleteName(personName['.key'])">Delete</v-btn>
          </v-flex>
          <v-flex xs12 sm6 offset-sm3 v-else>
            <v-card>
              <v-text-field v-model="personName.name"></v-text-field>
              <v-btn @click="saveEdit(personName)">Save</v-btn>
              <v-btn @click="cancelEdit(personName['.key'])">Cancel</v-btn>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { table } from './firebase'

export default {
  name: 'App',
  
  data () {
    return {
      name: '',
    }
  },

  firebase: {
    names: table
  },

  methods: {
    submitName(){
      table.push({ name: this.name, edit: false })
      this.name = ''
    },
    deleteName(key){
      table.child(key).remove()
    }, 
    editName(key){
      table.child(key).update({edit: true})
    },
    cancelEdit(key){
      table.child(key).update({edit: false})
    },
    saveEdit(person){
      const key = person['.key']
      table.child(key).set({ name: person.name, edit: false })
    }
  }
}
</script>
