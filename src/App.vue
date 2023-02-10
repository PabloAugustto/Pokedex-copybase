<template>
  <v-app></v-app>
</template>

<script>
export default {
  name: 'App',

  components: {},

  data: () => ({
    //
  }),

  mounted(){
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=493")
      .then((response) => {
        this.pokemons = response.data.results;
      });
  },
  methods: {
    show_pokemon(id) {
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
        this.selected_pokemon = response.data;
        this.show_dialog = !this.show_dialog;
      });
    },
    get_move_level(move) {
      for (let version of move.version_group_details) {
        if (
          version.version_group.name == "sword-shield" &&
          version.move_learn_method.name == "level-up"
        ) {
          return version.level_learned_at;
        }
      }
      return 0;
    },
  },
  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
    },
  },
};
</script>

<style>
#app {
background: linear-gradient(
  to bottom right,
  rgba(10, 10, 10, 1),
  rgba(12, 39,63, 1)
)
  no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size:cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
</style>
