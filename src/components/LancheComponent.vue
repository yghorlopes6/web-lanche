<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <div class="row mt-2">
          <div class="col">
            <img :src="pao[0]" />
            <img :src="alface" />
            <img :src="ketchup" />
            <img :src="mostarda" />
            <img :src="maionese" />
            <img :src="hamburguer" />
            <img :src="pao[1]" />
          </div>
        </div>
      </div>
      <div class="col-md-6" v-if="etapa == 1">
        <div class="row mt-2">
          <paoComp />
        </div>

        <div class="row mt-2">
          <SaladaComp />
        </div>

        <div class="row mt-2">
          <MolhoComp />
        </div>

        <div class="row mt-2">
          <HamburguerComp />
        </div>
        <div class="row mt-2">
          <div class="col d-flex justify-content-end">
            <button type="button" class="btn btn-info" @click="fazerPedido()">
              Fazer pedido
            </button>
          </div>
        </div>
      </div>
      <div class="col-md-6" v-if="etapa == 2">
        <div class="row mt-2">
          <div class="col">
            <div class="card">
              <div class="card-header">Dados</div>
              <div class="card-body">
                <div class="mb-3">
                  <label class="form-check-label">Nome</label>
                  <input class="form-control" type="text" v-model="inputNome" />
                </div>
                <div class="mb-3">
                  <label class="form-check-label">Endereço</label>
                  <input
                    class="form-control"
                    type="text"
                    v-model="inputEndereco"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="row mt-2">
          <div class="col d-flex justify-content-between">
            <button type="button" class="btn-warning btn" @click="etapa -= 1">
              Voltar
            </button>

            <button
              type="button"
              class="btn-success btn"
              @click="confirmarPedido()"
            >
              Confirmar
            </button>
          </div>
        </div>
      </div>
      <div class="col-md-6" v-if="etapa == 3">
        <div class="row mt-2">
          <div class="col">
            <div class="card">
              <div class="card-header">Pedido Confirmado</div>
              <div class="card-body">
                <p>Agora é só aguardar 10 minutos!</p>
                <p>Vamos chamar você em breve :)</p>
              </div>
            </div>
          </div>
        </div>
        <div class="row mt-2">
          <div class="col d-flex justify-content-between">
            <button type="button" class="btn-primary btn" @click="etapa = 1">
              Repetir pedido
            </button>

            <button type="button" class="btn-success btn" @click="novoPedido()">
              Novo pedido
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import paoComp from "./PaoComponent.vue";
import SaladaComp from "./SaladaComponent.vue";
import MolhoComp from "./MolhoComponent.vue";
import HamburguerComp from "./HamburguerComponent.vue";
export default {
  data() {
    return {
      inputTipoPao: "",
      inputSalada: [],
      inputMolho: [],
      inputHamburguer: "",
      etapa: 1,
      inputNome: "",
      inputEndereco: "",
      novoPedidoAssincrono: null,
    };
  },
  computed: {
    pao() {
      switch (this.inputTipoPao) {
        case "gergelim":
          return [
            require("@/assets/imagens/pao_gergelim_superior.png"),
            require("@/assets/imagens/pao_gergelim_inferior.png"),
          ];
        case "australiano":
          return [
            require("@/assets/imagens/pao_australiano_superior.png"),
            require("@/assets/imagens/pao_australiano_inferior.png"),
          ];
        default:
          return [
            require("@/assets/imagens/padrao/pao_superior.png"),
            require("@/assets/imagens/padrao/pao_inferior.png"),
          ];
      }
    },
    alface() {
      return this.inputSalada.includes("alface")
        ? require("@/assets/imagens/alface.png")
        : require("@/assets/imagens/padrao/alface.png");
    },
    ketchup() {
      return this.inputMolho.includes("ketchup")
        ? require("@/assets/imagens/ketchup.png")
        : require("@/assets/imagens/padrao/molho.png");
    },
    maionese() {
      return this.inputMolho.includes("maionese")
        ? require("@/assets/imagens/maionese.png")
        : require("@/assets/imagens/padrao/molho.png");
    },
    mostarda() {
      return this.inputMolho.includes("mostarda")
        ? require("@/assets/imagens/mostarda.png")
        : require("@/assets/imagens/padrao/molho.png");
    },
    hamburguer() {
      switch (this.inputHamburguer) {
        case "bovino":
          return require("@/assets/imagens/bovino.png");
        case "frango":
          return require("@/assets/imagens/frango.png");
        case "soja":
          return require("@/assets/imagens/soja.png");
        default:
          return require("@/assets/imagens/padrao/hamburguer.png");
      }
    },
  },
  methods: {
    fazerPedido() {
      if (this.inputTipoPao && this.inputHamburguer) {
        this.etapa = 2;
      } else {
        alert("Você precisa selecionar no mínimo um pão e um hamburguer");
      }
    },
    confirmarPedido() {
      if (this.inputNome && this.inputEndereco) {
        this.etapa = 3;
        this.novoPedidoAssincrono = setTimeout(() => this.novoPedido(), 7000);
      } else {
        alert("Você precisa informar seus dados");
      }
    },
    novoPedido() {
      this.etapa = 1;
      this.inputTipoPao = "";
      this.inputSalada = [];
      this.inputMolho = [];
      this.inputHamburguer = "";
      this.inputNome = "";
      this.inputEndereco = "";
    },
  },
  watch: {
    etapa(novo) {
      if (novo == 1) {
        clearTimeout(this.novoPedidoAssincrono);
      }
    },
  },
  components: {
    paoComp,
    SaladaComp,
    MolhoComp,
    HamburguerComp,
  },
};
</script>
<style>
/* Estilos do componente LancheComponent */
</style>
