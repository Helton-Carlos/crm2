<template>
  <q-card
    class="my-card-syclus"
    flat
    bordered
    :style="{ width: `${width}`, height: `${height}` }"
  >
    <q-item class="items-center topo-fixo" :class="cor_header" dense="dense">
      <q-item-section
        style="height:40px;font-weight:700;color:White;padding-left:10px"
      >
        {{ card }}
      </q-item-section>
      <q-btn
        v-if="btn_comando === 'btn-atualizar'"
        round
        flat
        text-color="white"
        icon="autorenew"
        @click.prevent="atualizarConteudo"
      >
      </q-btn>
    </q-item>
    <q-card-section class="corpo">
      <div
        v-for="(grupos, indexGrupo) in this.ObjConteudo.grupos"
        :key="indexGrupo"
      >
        <div class="spin" style="width:230px" v-show="carregarKnob">
          <q-knob
            v-model="value"
            size="30px"
            :thickness="0.4"
            color="primary"
            track-color="cyan-3"
          />
        </div>
        <div
          v-show="carregarText"
          style="margin:0 auto;text-align:center;padding-top:20px;color:red"
        >
          <span>Não possui grupos...</span>
        </div>
        <div
          class="flex justify-between items-center q-my-none hover"
          style="padding:10px"
        >
          <a
            class="text-dark"
            @click.prevent="abrirItem(indexGrupo, indexItem)"
          >
            {{ grupos.grupo }}
          </a>
          <div class="text-blue-grey-7" style="font-size:12.5px">
            <q-img :src="url" />
            {{ this.formataCaptionGrupo(grupos.qtde) }}
          </div>
        </div>
        <div style="width:95%;margin-left:15px">
          <q-separator />
        </div>
      </div>
    </q-card-section>
  </q-card>
</template>

<script>
import importSql from "app/src/commands/importSql";
export default {
  mixins: [importSql],
  props: [
    "idPrincipal",
    "conteudo_card",
    "card",
    "btn_comando",
    "cor_header",
    "formato_card",
    "msg",
    "link_item",
    "width",
    "height"
  ],
  data() {
    return {
      url: "../assets/syclus.png"
    };
  }
};
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
}
.margin-bot {
  margin-bottom: 3px;
}
.pl {
  padding-left: 5px;
}
.hover:hover {
  background-color: rgb(205, 205, 205);
}
.my-card-s {
  width: 450px;
}
.corpo {
  padding: 0;
  margin: 0px auto;
  max-height: 250px;
  overflow: auto;
}
.text-class {
  font-weight: 400;
  font-style: italic;
  padding-left: 15px;
  width: 90%;
  cursor: pointer;
}
.text-class:hover {
  color: rgb(11, 187, 218);
  transition: 0.5s;
}
.spin {
  text-align: center;
  margin: 30px auto;
  animation: spins 1s infinite;
}

@keyframes spins {
  to {
    transform: rotate(360deg);
  }
}
@media only screen and (max-width: 1320px) {
  .my-card-syclus {
    width: 350px;
  }
}
</style>
