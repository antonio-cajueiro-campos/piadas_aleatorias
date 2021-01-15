<template>
  <div class="" id="app">
    <header class="navbar">
      <section class="navbar-section">
        <h1 class="titulo">APIadas aleatórias (entendeu?)</h1>
      </section>
    </header>
    <div class="container grid-xs py-2">
      <form @submit.prevent="show()">
        <button v-if="piada == ''" class="btn btn-primary input-group-btn initBtn">Clique em mim para rir!</button>
        <div v-if="piada">
          <div class="line"></div>
          <div class="joke">
            {{ piada }}
          </div>
          <div class="line"></div>
          <input type="submit" class="btn btn-primary input-group-btn nextJk" value="Próxima piada!">
          <!--<button type="button" class="btn btn-outline-primary input-group-btn saveJk" @click="saveJoke()">Salvar piada!</button>
          <button v-if="saved.length != 0 && savedOn == false" type="button" class="btn btn-outline-primary input-group-btn savedJk" @click="savedList()">Mostrar piadas salvas</button>
          <button v-if="saved.length != 0 && savedOn" type="button" class="btn btn-outline-primary input-group-btn savedJk" @click="savedList()">Ocultar piadas salvas</button> -->
        </div>
      </form>
      <!-- <div class="saved">
        <PiadasSalvas v-if="savedOn" />
      </div> -->
    </div>
    <footer>
      <p>Antonio Carlos - <a href="https://github.com/antonio-cajueiro-campos">GitHub</a> | Powered By <a target="_blank" href="https://icanhazdadjoke.com">icanhazdadjoke.com</a></p>
    </footer>
  </div>
</template>

<script>
//import PiadasSalvas from "./components/piadasSalvas";
export default {
  name: 'App',
  //components: { PiadasSalvas },
  data() {
    return {piada: "", json: null, saved: [], savedOn: false};
  },
  methods: {
    savedList() {
      
    },

    //salva a piada num array para exibir posteriormente em um componente
    saveJoke() {
     
      this.saved.push(this.piada)
      this.saved = this.saved.filter(function(elem, index, self) {
        return index === self.indexOf(elem);
      });
    },

    // método para obter o json da api através do fetch API
    async getJson() {
      const url = `https://icanhazdadjoke.com/slack`;
      this.json = await fetch(url).then(response => response.json());
    },
  
    // método para mostrar a piada
    async show() {
      let newJoke;

      // verifica se o json recebido não está no estado inicial de nulo
      if (this.json != null) {
        newJoke = this.json.attachments[0].fallback;

        // verifica se o json recebido é igual ao anterior para evitar que piada se reptia duas vezes consecutivas
        if (newJoke != this.piada)
          this.piada = newJoke;
        else {
          await this.getJson();
          this.show();
        }
      } else {
        await this.getJson();
        this.show();
      }
    }
  }
}




</script>

<style>
.joke {
  font-size: 20px;
}
.line {
  height: 0px;
  width: 100%;
  margin: 10px 0;
  border-bottom: black 3px dotted;
}
.titulo {
  color: white;
  font-family: cursive;
  margin: 15px auto;
  font-size: 28px;
}
header {
  background: rgb(206, 206, 206);
}
.initBtn, .nextJk, .saveJk, .savedJk {
  position: absolute;
  top: 300px;
  left: 50%;
  transform: translate(-50%);
}
.nextJk {
  left: 42%;
}
.saveJk {
  left: 58%;
}
.savedJk {
  top: 410px;
}
footer {
  z-index: 5;
  position: absolute;
  bottom: 0;
  background: rgb(199, 199, 199);
  width: 100%;
  height: 50px;

}
footer > p {
  font-weight: bold;
  margin: auto 20px;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
</style>
