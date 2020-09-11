<template>
  <div id="contenedor">
    <div id="cabecera">
      <img :src="logo" alt width="300" />
      <p id="indicaciones">Ingresa nombre o número de índice de tu Pokémon</p>
      <div id="ingreso-datos">
        <input v-model="nombrePokemon" type="text" @keyup.enter="traerPokemon" />
        <button id="boton" @click="traerPokemon">Pokémon</button>
      </div>
    </div>
    <div id="pokemon-obtenido">
      <div id="movimientos">
        <ol>
          <li
            v-for="(movimiento, index) in movimientos"
            :key="index"
          >{{movimiento.move.name.toUpperCase()}}</li>
        </ol>
      </div>

      <div id="nombre-imagen">
        <img :src="imagen" alt width="200" />
        <h2>{{nombre}}</h2>
        <p>{{cantidad}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokéApi",
  data() {
    return {
      nombrePokemon: "",
      pokemon: {
        name: "",
        sprites: { front_default: "" },
        moves: [],
        cantidad: "",
      },
    };
  },
  created() {
    this.traerPokemon();
  },
  methods: {
    traerPokemon() {
      fetch(this.url)
        .then((response) => {
          return response.json();
        })
        .then((myJson) => {
          console.log(myJson);
          this.pokemon = myJson;
          this.nombrePokemon = '';
        });
    },
  },
  computed: {
    logo() {
      return "https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1920px-International_Pok%C3%A9mon_logo.svg.png";
    },
    imagen() {
      return this.pokemon.sprites.front_default;
    },
    nombre() {
      return this.pokemon.name.toUpperCase();
    },
    movimientos() {
      return this.pokemon.moves;
    },
    cantidad() {
      return (this.cantidad = `${this.pokemon.moves.length} Movimientos`);
    },
    url() {
      return this.nombrePokemon == ""
        ? `https://pokeapi.co/api/v2/pokemon/pikachu`
        : `https://pokeapi.co/api/v2/pokemon/${this.nombrePokemon}`;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  font-family: "Lexend Peta", sans-serif;
  color: #2b73b9;
  margin-top: 0;
}
h1 {
  font-family: "Poppins", sans-serif;
  margin: 30px;
}
#indicaciones {
  margin-top: 20px;
} 
#boton {
  background-color: #2d70b7;
  color: #ffcb05;
  border-top-right-radius: 4px;
  border-bottom-right-radius: 4px;
  border: 1px solid #21386e;
}
#boton:hover {
  background-color: #21386e;
}
#contenedor {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  margin-top: 0;
}
#cabecera {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}
#ingreso-datos {
  display: flex;
  justify-content: center;
  width: 300px;
  margin: 20px;
}
#pokemon-obtenido {
  display: flex;
  justify-content: space-around;
  margin-top: 30px;
}
#nombre-imagen {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 300px;
  background-image: url('https://us.123rf.com/450wm/apostrophe/apostrophe1410/apostrophe141000015/32373880-fondo-azul-abstracto-color-de-invierno-centro-blanco-marco-oscuro-esponja-desva%C3%ADda-suave-dise%C3%B1o-de-textur.jpg?ver=6');
  border: 5px solid #ffcb05;
  border-radius: 6px;
  box-shadow: 10px 10px 2px -6px rgba(42,116,186,1);
}
#movimientos {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
li {
  font-size: 14px;
}
</style>