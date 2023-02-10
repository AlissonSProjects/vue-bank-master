<template>
  <v-container>
    <v-card elevation="3">
      <v-card-title>VUE BANK</v-card-title>
      <v-card-item >
        <v-img class="rounded-lg" src="../src/VueBank.png" width="210px" ></v-img>
      </v-card-item>

      <v-form class="ma-5 d-flex">
        <v-text-field label="Favorecido" variant="outlined" v-model="conta.favorecido" type="text" inputmode="" />
        <v-text-field label="Agencia" variant="outlined" v-model="conta.agencia" type="number" inputmode="numeric" min="0" maxlength="4"/>
        <v-text-field label="Conta" variant="outlined" v-model="conta.numero" type="number" inputmode="numeric" min="0" maxlength="8"/>
        <v-btn class="mt-2 ms-5" color="indigo" @click="criarConta">Criar</v-btn>
      </v-form>
    </v-card>
    <div class="d-flex justify-center">

      <v-card elevation="3" class="mt-5 mx-3" v-for="conta in listaDeContas" :key="conta.numero">
        <v-card-title class="font-weight-bold">{{ conta.favorecido }} <v-spacer /> R$ {{ conta.saldo }}</v-card-title>
        <v-card-actions>
          <v-text-field label="Valor" v-model="valor" type="number" step="1" min="0"/>
          <v-btn color="green" @click="depositar(conta, valor)">Depositar</v-btn>
          <v-btn color="red" @click="sacar(conta, valor)">Sacar</v-btn>
          <v-btn color="black" @click="removerConta(conta)">Sair</v-btn>
        </v-card-actions>
      </v-card>
    </div>
      
    <v-snackbar
      v-model="mostrarSnackbar"
      location="top right"
    >
      Conta criada com sucesso

      <template v-slot:actions>
        <v-btn
          color="pink"
          variant="text"
          @click="mostrarSnackbar = false"
        >
          Fechar
        </v-btn>
      </template>
    </v-snackbar>

    <v-snackbar
      v-model="mostrarMensagemErro"
      location="center"
    >
      Saldo insuficiente

      <template v-slot:actions>
        <v-btn
          color="pink"
          variant="text"
          @click="mostrarSnackbar = false"
        >
          Fechar
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      listaDeContas: [],
      mostrarSnackbar: false,
      conta: {
        favorecido: "",
        agencia: "",
        numero: "",
        saldo: 0
      },
      valor: 0
    };
  },
  methods: {
    criarConta() {
  if (this.listaDeContas.length === 0) {
    if (this.conta.favorecido !== "" && this.conta.agencia !== "" && this.conta.numero !== "") {
      
      if (this.conta.favorecido.match(/\d+/g) === null) {
        this.mostrarSnackbar = true;
        this.listaDeContas.push({ ...this.conta });
      } else {
        alert("O campo 'Favorecido' não pode conter números");
      }
    } else {
      alert("O nome do favorecido, a agência e o número da conta são obrigatórios");
    }
  } else {
    alert("A conta já foi criada");
  }
},
    depositar(conta) {
      conta.saldo += Number(this.valor);
    },
    sacar(conta) {
      if (conta.saldo >= Number(this.valor)) {
        conta.saldo -= Number(this.valor);
      } else {
        alert("Saldo insuficiente");
      }
    },
    removerConta(conta) {
      let index = this.listaDeContas.indexOf(conta);
      if (index !== -1) {
        this.listaDeContas.splice(index, 1);
      }
    }
  }
};

</script>