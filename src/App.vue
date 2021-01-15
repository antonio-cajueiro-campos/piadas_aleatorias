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
          {{ piada }}
          <div class="line"></div>
          <input type="submit" class="btn btn-primary input-group-btn nextJk" value="Próxima piada!">
        </div>
      </form>
    </div>
    <footer>
      <p>Antonio Carlos - <a href="https://github.com/antonio-cajueiro-campos">GitHub</a></p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {piada: "", json: null};
  },
  methods: {

    // método para obter o json da api
    async getJson() {
      const url = `https://icanhazdadjoke.com/slack`;
      this.json = await fetch(url).then(async(response) => {
        const data = await response.json();
        return data;
      });
    },
  
    // método para mostrar a piada
    async show() {
      var newJoke;

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
.initBtn, .nextJk {
  position: absolute;
  top: 200px;
  left: 50%;
  transform: translate(-50%);
}
footer {
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
