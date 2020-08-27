<template>
  <div id="app">
    <div class="flex w-1/2 m-auto mt-20">
      <div
        class="w-full border-b-4 pb-4 border-green-500 text-3xl font-medium mb-8 text-gray-700"
      >
        List of Passengers
      </div>
    </div>
    <div
      class="flex flex-wrap w-1/2 m-auto p-10 my-10 overflow-y-scroll rounded-lg  shadow-lg"
      style="height: 60vh;"
    >
      <div class="w-full mh-full">
        <passenger
          v-for="passenger in passenger"
          :key="passenger._id"
          :data="passenger"
        ></passenger>
      </div>

      <div class="w-full">
        <infinity-loader
          :loading="loading"
          :onScroll="onScroll"
        ></infinity-loader>
      </div>
    </div>
  </div>
</template>

<script>
import InfinityLoader from "@/components/InfinityLoader.vue";
import Passenger from "@/components/Passenger.vue";
export default {
  name: "App",
  components: {
    InfinityLoader,
    Passenger,
  },
  data() {
    return {
      apiURL: "https://api.instantwebtools.net/v1/passenger",
      passenger: [],
      meta: {},
      params: {
        page: 0,
        size: 10,
      },
      loading: false,
    };
  },
  // Ao montar o objeto carregue os dados
  mounted() {
    this.getPassengers();
  },

  methods: {
    async getPassengers() {
      try {
        // Configurando os dados da requisição
        const url = new URL(this.apiURL);
        url.search = new URLSearchParams(this.params);

        // Exibindo o carregamento
        this.loading = true;

        // Realizando a Requisição
        fetch(url)
          .then((response) => response.json())
          .then((response) => {
            // Desestruturando os dados da response
            const { totalPassengers, totalPages, data } = response;
            // Caso já exista dados, unir os arrays em um
            if (this.passenger.length) {
              this.passenger = [...this.passenger, ...data];
            }
            // Caso não exista dados setar o valor que vem da requisição
            else {
              this.passenger = data;
            }
            // Seta os dados da paginação
            this.meta = {
              totalPassengers,
              totalPages,
            };
          });
      } catch (erro) {
        console.log(erro);
      } finally {
        this.loading = false;
      }
    },

    onScroll() {
      // Caso a página corrente seja menor que o total das páginas de dados vindas do servidor,
      // incremente o parametro da página e realize a requisição
      if (this.params.page < this.meta.totalPages) {
        this.params.page++;
        this.getPassengers();
      }
    },
  },
};
</script>

<style lang="css">
@import url("https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css");
</style>
