<template>
  <v-app>
    <v-card>
      <v-card-title>
        <h1 outlined color="black">Search Beer</h1>
        <v-spacer></v-spacer>
        <v-text-field
          type="text"
          v-model.trim="searchInqury"
          placeholder="Enter beer name here"
          @keyup="getAllBeers"
        />
      </v-card-title>
      <v-card-text v-if="searchInqury === ''">
        Enter a word into the search bar!
      </v-card-text>
      <v-list width="40%" class="d-flex flex-row m-9" v-else-if="beers.length">
        <v-list-item class="m-9" v-for="b in beers" :key="b.id">
          <v-list class="w-2">
            <v-img
              v-bind:src="b.image_url"
              alt="beer-img"
              max-width="50"
              height="auto"
            >
            </v-img>
            <h3>{{ b.name }}</h3>
            <v-list-item-text>{{ b.description }}</v-list-item-text>
          </v-list>
        </v-list-item>
      </v-list>
      <v-card-text v-else>
        No beer found! Try another word in search!
      </v-card-text>
    </v-card>
  </v-app>
</template>

<script>
export default {
  name: "Beer-Search",
  data() {
    return {
      beers: [],
      searchInqury: "",
    };
  },
  methods: {
    getAllBeers() {
      const url = "https://api.punkapi.com/v2/beers";
      fetch(url)
        .then((response) => response.json())
        .then((res) => {
          if (this.searchInqury) {
            this.beers = res.filter((beer) =>
              beer.name.toLowerCase().includes(this.searchInqury.toLowerCase())
            );
          } else {
            this.beers = res;
          }
        });
    },
    created() {
      this.getAllBeers();
    },
  },
};
</script>
