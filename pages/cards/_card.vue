<template>
  <div>
      <v-container>
        <v-row v-if="!isLoading">
          <v-col cols="12" md="4">
            <v-img
              :src="card.imageUrl"
              max-width="278.75"
              max-height="387.5"
              @click="openImage = card"
            ></v-img>
            <v-overlay v-if="openImage" @click="openImage = false">
              <v-img
                :src="card ? card.imageUrl : ''"
                width="446"
                height="620"
              ></v-img>
            </v-overlay>
          </v-col>
          <v-col cols="12" md="8">
            <h1>{{ card.name }}</h1>
            <ManaCost v-bind:manaCost="card.manaCost"></ManaCost>
            <v-list>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title><label>Type :</label> {{ card.type }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title><label>Set name :</label> {{ card.setName }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
            <p class="description">
              {{ card.text }}
            </p>
            <p class="citation" v-if="card.flavor">
              "{{ card.flavor }}"
            </p>
            <v-list-item-action>
              <v-icon large=true>mdi-plus-circle</v-icon>
            </v-list-item-action>
          </v-col>
        </v-row>
      </v-container>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        isLoading: false,
        card: {},
        openImage: null
      }
    },
    created () {
      this.getCardById()
    },
    methods: {
      async getCardById () {
        this.isLoading = true
        fetch(`https://api.magicthegathering.io/v1/cards/${this.$route.params.card}`)
        .then(res => res.clone().json())
        .then(res => {
          this.card = res.card
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
  label {
    color: #888;
  }
  .description {
    margin: 30px 0;
  }
  .citation {
    font-style: italic;
  }
  .mana-cost {
    margin-bottom: 30px;
  }
</style>
