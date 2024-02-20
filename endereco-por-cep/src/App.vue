<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col>
            <h1 class="text-uppercase">Busque endereço pelo CEP</h1>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="11">
            <v-text-field
              dense
              maxlength="8"
              label="Digite o CEP desejado ..."
              v-model="cep"
            ></v-text-field>
          </v-col>
          <v-col cols="1">
            <v-btn
              color="primary"
              icon="mdi-home-search-outline"
              @click="buscarEndereco"
            ></v-btn>
          </v-col>
        </v-row>

        <v-row v-if="endereco.logradouro !== ''">
          <v-col>
            <v-card>
              <v-card-title>Resultado</v-card-title>
              <v-card-text>
                <p>Logradouro: {{ endereco.logradouro }}</p>
                <p>Bairro: {{ endereco.bairro }}</p>
                <p>Cidade: {{ endereco.cidade }}</p>
                <p>Estado: {{ endereco.estado }}</p>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
//
export default {
  name: "App",
  data: () => ({
    //
    cep: "",
    endereco: {
      logradouro: "",
      bairro: "",
      cidade: "",
      estado: "",
    },
  }),
  methods: {
    buscarEndereco() {
      if (this.cep.length !== 8) {
        alert("CEP inválido");
        return;
      } else {
        fetch(`https://viacep.com.br/ws/${this.cep}/json/`)
          .then((response) => {
            if (!response.ok) {
              throw new Error("Erro ao buscar o endereço");
            }
            return response.json();
          })
          .then((data) => {
            this.endereco.logradouro = data.logradouro;
            this.endereco.bairro = data.bairro;
            this.endereco.cidade = data.localidade;
            this.endereco.estado = data.uf;
          })
          .catch((error) => {
            console.error("Erro:", error);
          });
      }
    },
  },
};
</script>
