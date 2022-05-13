
<template>

  <h1>Tabela Amortização</h1>
  <div class="container">
    <div class="row">
      <div class="box">
      <label class="label">Montante</label>
      <input class="input" type="text" v-model="montante">
      <label class="label">Juros (%)</label>
      <input class="input" type="text" v-model="jurosTotal">
      <label class="label">Parcelas (Mensais)</label>
      <input class="input" type="text" v-model="parcelasTotais">
      <button class="button" @click="calc_pagamentos" >
      Calcular
    </button>
    </div>
    </div>
    </div>
    
    <table class="table" cellspacing="10" v-if="pagamentos.length">
      <thead>
        <tr>
          <th>Parcelas</th>
          <th>Pagamento</th>
          <th>Juros</th>
          <th>Descontado</th>
          <th>Dívida</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pagamento in pagamentos" :key="pagamento.parcelas">
          <td>{{ pagamento.parcelas }}</td>
          <td>{{ "R$ "+pagamento.pagamento.toFixed(2) }}</td>
          <td>{{ "R$ "+pagamento.juros.toFixed(2) }}</td>
          <td>{{ "R$ "+pagamento.descontado.toFixed(2) }}</td>
          <td>{{ "R$ "+ pagamento.divida.toFixed(2) }}</td>
        </tr>
      </tbody>
    </table>
  


  
</template>

<script>
  export default {
    data() {
      return {
        montante: 500000,
        parcelasTotais: 0,
        jurosTotal: 0,
        pagamentos: [],
      };
    },
    methods: {
      calculo() {
        this.montante = "";
        this.parcelasTotais = "";
        this.jurosTotal = "";
        this.pagMensal = "";
        this.pagamentos = [];
      },

      calc_valorParcelas( p = this.montante, n = this.parcelasTotais, int = this.jurosTotal){
        let r = int / 100 / 12;
        let pmt = p * ((r * Math.pow(1 + r, n)) / (Math.pow(1 + r, n) - 1));
        this.pagMensal = pmt;

        return pmt;
      },

      calc_pagamentos() {
        let divida = Number(this.montante)
       let parcelas = 0;
        var pagamentos = [];
        let pagamento = Number(this.calc_valorParcelas());
        do {
          parcelas++;
          var juros = (divida * (Number(this.jurosTotal)/12/100))
          var descontado = pagamento - juros;

          if (divida < pagamento ){
            descontado = divida;
            pagamento = (Number(juros) + Number(divida))
          }

          divida = divida - descontado;

          pagamentos.push({
            divida: divida,
            pagamento: pagamento,
            juros: juros,
            descontado: descontado,
            parcelas,
          });
        } while (divida > 0);
        this.pagamentos = pagamentos;
      },
    },
  };
</script>

<style>

:root{
  --clr-light: white;
  --clr-dark: #90a4ae;
  --clr-dark-heavy: #546e7a;

}
h1{
  text-align: center;
  font-family: 'Montserrat', sans-serif;
}
.container{
  display: flex;
  justify-content: center;
  align-items: center;
}
body{
  background-color: #ecf0f3;
}

input {
  max-width: 150px;
  height: 24px;
  margin-top: 9px;
  border: none;
  outline: none;
  background: none;
  font-size: 15px;
  color: var(--clr-dark);
  box-shadow: inset 8px 8px 8px #cbced1, inset -8px -8px 8px #ffffff;
  border-radius: 25px;
  text-align: center;

}
.box .label, .box .button{
  display: flex;
  flex-direction: column;
  text-align: center;
  margin-top: 25px;
}
.box .label{
  font-size: 16px;
}
.row {
  width: 330px;
  height: 270px;
  padding: 60px 21px 60px 35px;
  border-radius: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ecf0f3;
  box-shadow: 13px 13px 20px #cbced1, -13px -13px 20px #fff;
  font-family: 'Montserrat', sans-serif;
  font-weight: bold;
  color: var(--clr-dark-heavy);
  margin-top: 9px;
}
.button{
  width: 6em;
  height: 2em;
  margin: 20px 0 0 28px;
  font-size: 1em;
  border-radius: 5em;
  border: none;
  outline: none;
  box-shadow: -5px -5px 10px var(--clr-light), 5px 5px 10px var(--clr-dark);
  color: var(--clr-dark-heavy);
  justify-content: center;
  align-items: center;
  font-weight: bold;
  cursor: pointer;
}
.table{
  margin-top: 5%;
  border-top: 1px solid #ccc;
  width: 100%;
  border-collapse: collapse;
  overflow: hidden;
  border-radius: 10px;
}
th{
    
    font-size: 20px;
    color: #fff;
    line-height: 1.2;
    font-weight: unset;
}
table tbody tr{
  font-size: 18px;
  line-height: 3.2;
  font-weight: bold;
  text-align: center;
  border: 1px solid rgb(71, 70, 70);
}
table thead tr{
  background: #36304a;
  height: 60px;
}
tbody tr:nth-child(odd){
  background-color: #ccc;
}
</style>
