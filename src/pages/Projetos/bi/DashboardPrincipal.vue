<template>
  <div>
    <BarraLayout
      @OnClick="OnClickBarraLayout"
      :ConteudoBtn="this.ObjDashboard['grupos']"
    />
    <div class="row">
      <div
        v-for="ObjCard in this.ObjDashboard.grupos[this.IndexGrupoAtual].cards"
        :key="ObjCard"
        style="margin:5px;margin-bottom:5px;"
      >
        <CardComparativoApi
          v-if="ObjCard.tipo_card === 'CardComparativoApi'"
          class="q-ma-xs"
          :id="ObjCard.id_card"
          :card="ObjCard.card"
          :ordem="ObjCard.ordem"
          cor_header="bg-primary"
          topo_fixo="topo_fixo"
          :height="ObjCard.height"
          :style="{ width: `${ObjCard.width}` }"
          :btn_comando="ObjCard.btn_comando"
          :tipo_card="ObjCard.tipo_card"
          :sub_tipo="ObjCard.sub_tipo"
          :conteudo_card="ObjCard.conteudo_card"
          :link_item="ObjCard.link_item"
          :idPrincipal="this.idColaboradorAtivo"
          :msg="this.msgCard"
        />

        <CardGrupoApi
          v-if="ObjCard.tipo_card === 'CardGrupoApi'"
          class="q-ma-xs"
          :id="ObjCard.id_card"
          :card="ObjCard.card"
          :ordem="ObjCard.ordem"
          cor_header="bg-primary"
          topo_fixo="topo_fixo"
          :height="ObjCard.height"
          :style="{ width: `${ObjCard.width}` }"
          :btn_comando="ObjCard.btn_comando"
          :tipo_card="ObjCard.tipo_card"
          :sub_tipo="ObjCard.sub_tipo"
          :conteudo_card="ObjCard.conteudo_card"
          :link_grupo="ObjCard.link_grupo"
          :link_item="ObjCard.link_item"
          :idPrincipal="this.idColaboradorAtivo"
          :msg="this.msgCard"
        />

        <CardGraficoApi
          v-if="ObjCard.tipo_card === 'CardGraficoApi'"
          class="q-ma-xs"
          :id="ObjCard.id_card"
          :card="ObjCard.card"
          :ordem="ObjCard.ordem"
          cor_header="bg-primary"
          topo_fixo="topo_fixo"
          :height="ObjCard.height"
          :style="{ width: `${ObjCard.width}` }"
          :btn_comando="ObjCard.btn_comando"
          :tipo_card="ObjCard.tipo_card"
          :coluna_categoria="ObjCard.coluna_categoria"
          :coluna_serie="ObjCard.coluna_serie"
          :coluna_totalizadora="ObjCard.coluna_totalizadora"
          :sub_tipo="ObjCard.sub_tipo"
          :conteudo_card="ObjCard.conteudo_card"
          :link_item="ObjCard.link_item"
          :idPrincipal="this.idColaboradorAtivo"
          :msg="this.msgCard"
        />
        <CardListaApi
          v-if="ObjCard.tipo_card === 'CardListaApi'"
          class="q-ma-xs"
          :id="ObjCard.id_card"
          :card="ObjCard.card"
          :ordem="ObjCard.ordem"
          cor_header="bg-primary"
          :style="{ width: `${ObjCard.width}` }"
          topo_fixo="topo_fixo"
          :height="ObjCard.height"
          :width="ObjCard.width"
          :btn_comando="ObjCard.btn_comando"
          :tipo_card="ObjCard.tipo_card"
          :sub_tipo="ObjCard.sub_tipo"
          :conteudo_card="ObjCard.conteudo_card"
          :link="ObjCard.link"
          :idPrincipal="this.idColaboradorAtivo"
          :msg="this.msgCard"
        />
      </div>
    </div>
  </div>
</template>

<script>
import BarraLayout from "src/layouts/BarraLayout.vue";
import CardGrupoApi from "src/components/Cards/CardGrupoApi.vue";
import CardListaApi from "src/components/Cards/CardListaApi.vue";
import CardGraficoApi from "src/components/Cards/CardGraficoApi.vue";
import CardComparativoApi from "src/components/Cards/CardComparativoApi.vue";
import { GeLayoutDashBoard } from "src/commands/layoutDashboard.js";
import { defineComponent } from "vue";
import { ref } from "vue";
import { computed } from "vue";
import { useStore } from "vuex";

export default defineComponent({
  components: {
    BarraLayout,
    CardGrupoApi,
    CardListaApi,
    CardGraficoApi,
    CardComparativoApi
  },
  name: "dashboard",
  setup() {
    const $store = useStore();
    const login = computed({
      get: () => $store.state.showcase.login
    });
    const fabPos = ref([18, 18]);
    const draggingFab = ref(false);
    return {
      login,
      fabPos,
      draggingFab,
      moveFab(ev) {
        draggingFab.value = ev.isFirst !== true && ev.isFinal !== true;
        fabPos.value = [
          fabPos.value[0] - ev.delta.x,
          fabPos.value[1] - ev.delta.y
        ];
      }
    };
  },
  data() {
    return {
      ObjDashboard: [],
      IndexGrupoAtual: 0,
      Grupos: [],
      GrupoCards: [],
      GrupoCardsOpcionais: [],
      idColaboradorAtivo: 0
    };
  },
  methods: {
    OnClickBarraLayout(IndexGrupo) {
      this.IndexGrupoAtual = IndexGrupo;
      this.AtualizarCardsGrupoAtual();
    },
    AtualizarCardsGrupoAtual() {
      this.handleResize();
      this.msgCard = "atualizar_conteudo";
      setTimeout(() => {
        this.msgCard = "";
      }, 1000);
    },
    handleResize() {
      this.telaWidth = window.innerWidth;
      if (window.innerWidth <= 1006) {
        for (
          let i = 0;
          i < this.ObjDashboard.grupos[this.IndexGrupoAtual].cards.length;
          i++
        ) {
          this.ObjDashboard.grupos[this.IndexGrupoAtual].cards[i][
            "width"
          ] = `${this.telaWidth - 80}px`;
        }
      }
      if (window.innerWidth <= 797) {
        for (
          let i = 0;
          i < this.ObjDashboard.grupos[this.IndexGrupoAtual].cards.length;
          i++
        ) {
          this.ObjDashboard.grupos[this.IndexGrupoAtual].cards[i][
            "width"
          ] = `${this.telaWidth - 20}px`;
        }
      }
      if (window.innerWidth >= 797) {
        for (
          let i = 0;
          i < this.ObjDashboard.grupos[this.IndexGrupoAtual].cards.length;
          i++
        ) {
          this.ObjDashboard.grupos[this.IndexGrupoAtual].cards[i][
            "width"
          ] = this.ObjDashboard.grupos[this.IndexGrupoAtual].cards[i]["width"];
        }
      }
    }
  },
  beforeRouteEnter(to, from, next) {
    let login = JSON.parse(localStorage.getItem("login"));
    const permissao = login.aplicativos[0].recursos.dashboard_area_trabalho;
    if (!permissao) {
      next("/Login");
    }
    next();
  },
  created() {
    let login = JSON.parse(localStorage.getItem("login"));
    this.idColaboradorAtivo = login.id_colaborador;
    this.ObjDashboard = GeLayoutDashBoard(
      login.aplicativos[0].recursos.dashboard_area_trabalho.id_layout_dashboard
    );
    for (
      let i = 0;
      i <
      login.aplicativos[0].recursos.dashboard_area_trabalho
        .dashboard_complementar.length;
      i++
    ) {
      let ObjDashboardTemp = GeLayoutDashBoard(
        login.aplicativos[0].recursos.dashboard_area_trabalho
          .dashboard_complementar[i]
      );

      for (let j = 0; j < ObjDashboardTemp.grupos.length; j++) {
        this.ObjDashboard.grupos.push(ObjDashboardTemp.grupos[j]);
      }
    }
    this.msgCard = "limpar_conteudo";
    this.AtualizarCardsGrupoAtual();
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
  }
});
</script>
