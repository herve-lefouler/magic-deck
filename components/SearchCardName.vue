<template>
  <v-autocomplete
    v-model="model"
    :items="items"
    :loading="isLoading"
    :search-input.sync="search"
    clearable
    hide-details
    hide-selected
    item-text="name"
    item-value="id"
    label="Type a card name..."
    solo
  >
    <template v-slot:no-data>
      <v-list-item>
        <v-list-item-title>
          Search for your
          <strong>Cards</strong>
        </v-list-item-title>
      </v-list-item>
    </template>

    <template v-slot:item="{ item }">
      <v-container>
        <v-row>
          <v-col cols="12" md="10">
          <NuxtLink :to="'/cards/' + item.id">
              <v-list-item-content>
                <v-list-item-title v-text="item.name"></v-list-item-title>
                <v-list-item-subtitle>
                  <ManaCost v-bind:manaCost="item.manaCost"></ManaCost>
                </v-list-item-subtitle>
              </v-list-item-content>
          </NuxtLink>
        </v-col>
        <v-col cols="12" md="2">
        <v-list-item-action>
          <v-icon>mdi-plus-circle</v-icon>
        </v-list-item-action>
          </v-col>
        </v-row>
      </v-container>
    </template>
  </v-autocomplete>
</template>

<script>
import ManaCost from '../components/ManaCost'

export default {
  data: () => ({
    isLoading: false,
    items: [],
    model: null,
    search: null
  }),

  watch: {
    model (val) {
      if (val != null) this.tab = 0
      else this.tab = null
    },
    search (val) {
      //if (this.items.length > 0) return
      if (val.length < 3) return
      this.isLoading = true
      // Lazily load input items
      fetch('https://api.magicthegathering.io/v1/cards?name=' + val)
        .then(res => res.clone().json())
        .then(res => {
          this.items = res.cards
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => (this.isLoading = false))
    }
  }
}
</script>

<style scoped>
 a {
  color: #FFF;
  text-decoration: none;
 }
</style>
