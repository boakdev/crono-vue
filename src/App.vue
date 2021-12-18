<template>
  <div id="app">
    <img class="img" src="./assets/crono.png" />
    <a class="timer">{{ this.numero == 0 ? "00:00:00" : this.numero }}</a>
    <a class="miliseg" v-show="ms.length != 0">{{
      this.ms == 0 ? "000" : this.ms
    }}</a>

    <div class="areaBtn">
      <button class="botao" @click="vai">{{ botao }}</button>
      <button class="botao" @click="zerar">ZERAR</button>
    </div>

    <div class="lista" v-show="historico.length > 0">
      <ul>
        <li v-for="item in historico" :key="item">
          Você fez sua
          <strong>{{ historico.indexOf(item) + 1 }}ª pausa</strong> em:
          {{ item }}
        </li>
      </ul>
      <button @click="limparHistorico">Limpar histórico</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      numero: 0,
      botao: "VAI",
      timer: null,
      ms: 0,
      ss: 0,
      mm: 0,
      hh: 0,
      historico: [],
    };
  },

  methods: {
    vai() {
      if (this.timer != null) {
        clearInterval(this.timer);
        this.timer = null;
        this.botao = "VAI";
        if (this.ms != 0) {
          this.historico.push(this.numero + " ms:" + this.ms);
        }
      } else {
        this.timer = setInterval(() => {
          this.rodarTimer();
        }, 10);
        this.botao = "PAUSAR";
      }
    },
    zerar() {
      if (this.timer != null) {
        clearInterval(this.timer);
        this.timer = null;
      }
      this.ms = 0;
      this.ss = 0;
      this.mm = 0;
      this.hh = 0;
      this.numero = 0;
      this.botao = "VAI";
      this.limparHistorico();
    },

    rodarTimer() {
      this.ms += 10;

      if (this.ms == 1000) {
        this.ms = 0;
        this.ss++;
      }

      if (this.ss == 59) {
        this.ss = 0;
        this.mm++;
      }

      if (this.mm == 59) {
        this.mm = 0;
        this.hh++;
      }

      let format =
        (this.hh < 10 ? "0" + this.hh : this.hh) +
        ":" +
        (this.mm < 10 ? "0" + this.mm : this.mm) +
        ":" +
        (this.ss < 10 ? "0" + this.ss : this.ss);

      return (this.numero = format);
    },

    limparHistorico() {
      this.historico = [];
    },
  },
};
</script>

<style>
#app {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.img {
  width: 450px;
  height: 500px;
  padding-top: 100px;
}

.timer {
  color: #fff;
  font-size: 60px;
  margin-top: -250px;
}

.miliseg {
  color: #fff;
  font-size: 30px;
}

.areaBtn {
  margin-top: 200px;
  display: flex;
}

.botao {
  width: 150px;
  background-color: #fff;
  font-size: 20px;
  border: 0;
  border-radius: 5px;
  text-align: center;
  margin-left: 15px;
  margin-right: 15px;
  padding: 5px;
  cursor: pointer;
}

.botao:hover {
  opacity: 0.8;
  transition: all 0.5s;
}

ul {
  text-align: center;
  padding: 0px;
}

li {
  margin-top: 4px;
  padding: 15px;
  background-color: gray;
  list-style: none;
  color: #fff;
  font-size: 16px;
  border-radius: 5px;
}

.lista button {
  cursor: pointer;
  border: 0;
  background-color: #fff;
  padding: 8px;
  border-radius: 6px;
  margin-bottom: 10px;
}
</style>
