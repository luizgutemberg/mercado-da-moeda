<template>
  
  <div class=" container ">
      <div class="row">
        <h1 class="text-center"> Mercado da Moeda </h1>

        <input type="text"
          class="form-control text-light rounded-0 border-0 my-4"
          placeholder="Prourar por Moeda"
          @keyup="searchCoin()"
          v-model="textSearch"
        />

        <table class="table table-light">
          <thead>
            <tr>
              <th v-for="title in titles" :key="title">
              {{title}}
              
              </th>
            </tr>

          </thead>

          <tbody>
            <tr v-for="(coin, index) in filteredCoins" :key = "coin.id">
            <td class="text-muted">
              {{index + 1}}
            </td>

              <td>
                <img :src="coin.image" style="width:2rem" class="me-2">
                
                <span>
                  {{coin.name}}
                </span>

                <span class="ms-2 text-uppercase text-muted">
                  {{coin.symbol}}
                </span>
          
              </td>
              <td>
                ${{coin.current_price}}
              </td>

              <td
              :class="[
                coin.price_change_percentage_24h > 0
                ? 'text-success'
                : 'text-danger',
              ]"
              >

                {{coin.price_change_percentage_24h}} %
              
              </td>

              <td>
                ${{coin.total_volume.toLocaleString()}}
              </td>
            </tr>

          </tbody>
        </table>
      </div> 

      <span class="disclaimer"><a class="text-danger text-xs underline" >ISENÇÃO DE RESPONSABILIDADE IMPORTANTE:</a></span>


      <span class="text-lite text-xs"> Todos os conteúdos fornecidos no nosso website, sites hiperligados, aplicações associadas, fóruns, blogs, contas nas redes sociais e noutras plataformas ("website") são apenas para sua informação geral obtida por fontes de terceiros. Não fazemos garantias de qualquer tipo em relação ao nosso conteúdo, por exemplo, a precisão e atualização. Nenhuma parte do conteúdo que fornecemos constitui aconselhamento financeiro, jurídico nem qualquer outra forma de aconselhamento para a sua confiança específica por qualquer motivo. Qualquer utilização ou confiança no nosso conteúdo é unicamente da sua responsabilidade. Deve realizar a sua própria pesquisa, avaliação, análise e verificar o nosso conteúdo antes de confiar nele. Os câmbios são uma atividade de alto risco que pode levar a grandes perdas, portanto, consulte o seu consultor financeiro antes de tomar qualquer decisão. Nenhum conteúdo no nosso website é uma solicitação ou oferta.</span>


  </div>


</template>

<script>

export default {
  name: 'App',
  data() {
    return{
      coins: [],
      filteredCoins: [],
      titles:[ "#", "Coin", "Preço","Price Change","24h Volume"],
      textSearch: "",
        
    };

  },

  async mounted(){
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await res.json()
    console.log(data);
    this.coins = data
    this.filteredCoins = data
  },

  methods: {
    searchCoin(){
      this.filteredCoins = this.coins.filter((coin) => 
        coin.name.toLowerCase() .includes(this.textSearch.toLowerCase()) ||
        coin.symbol.toLowerCase() .includes(this.textSearch.toLowerCase())
      );


    },
  },
  

  };
</script>

<style>
</style>