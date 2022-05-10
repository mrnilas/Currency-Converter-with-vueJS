

<template>
  
  <main>
    <h1>Currency Converter</h1>
    <label for="input-euro">Euro</label><br>
    <input type="number" id="input-euro" name="input-euro" placeholder="1" v-model="wert_euro"  @change="setFromEuro"><br>
    <label for="input-CZK">Tschechische Kronen <span>(Rate: {{kurs_czk}})</span></label><br>
    <input type="number" id="input-CZK" name="input-CZK" placeholder="0" v-model="wert_czk" @change="setFromCZK"><br>
    <label for="input-CHF">Schweizer Franken <span>(Rate: {{kurs_chf}})</span></label><br>
    <input type="number" id="input-CHF" name="input-CHF" :placeholder="wert_chf" v-model="wert_chf" @change="setFromCHF"><br>
    <br>
    <hr>
    <br>
    <h3>Letztes Wechselkurs-Update: {{lastUpdate}}</h3>
  </main>
  <footer>
    build with <a href="https://getgeoapi.com">getgeoapi.com</a> an uploaded on <a href="#">GitHub</a>
  </footer>
</template>

<script>

export default{

  data(){
     return{
        data:[],
        currency_euro:"EUR",
        currency_czk:"CZK",
        currency_chf:"CHF",
        wert_euro:1,
        wert_czk:0,
        wert_chf:0,
        kurs_euro:0,
        kurs_czk:25,
        kurs_chf:1.04,
        lastUpdate:"API wurde nicht abgerufen :("
     }
  },

  methods:{

    fetchData(){
      const key = "e9f62102339f3e01d3efb7126722728992d10635";
      const url =`https://api.getgeoapi.com/v2/currency/convert?api_key=${key}&from=EUR&format=json`;

      fetch(url)
      .then(response=>response.json())
      .then(data=>{

        this.data=data;
        this.lastUpdate=data.updated_date;
        
        this.kurs_chf=data.rates.CHF.rate;
        

        if(data.status=="success"){
          console.log('success');
          console.log(this.kurs_czk);
          this.kurs_czk=data.rates.CZK.rate;
        }else{
          this.kurs_czk=25;
        }

        this.wert_czk=this.wert_euro*this.kurs_czk;
        this.wert_chf=this.wert_euro*this.kurs_chf;

        });

        
    },
    
    setFromEuro(k, wert_euro=+k.target.value ){
        //this.kurs_czk=data.rates.CZK.rate;
        this.wert_euro=wert_euro;
        this.wert_czk=(wert_euro*this.kurs_czk).toFixed(2);
        this.wert_chf=(wert_euro*this.kurs_chf).toFixed(2);
    },
    setFromCZK(k,wert_czk=+k.target.value){
        this.wert_czk=wert_czk;
        this.wert_euro=(wert_czk/this.kurs_czk).toFixed(2);
        this.wert_chf=(wert_czk/this.kurs_czk*this.kurs_chf).toFixed(2);
    },
    setFromCHF(k,wert_chf=+k.target.value){
      this.wert_chf=wert_chf;
      this.wert_euro=(wert_chf/this.kurs_chf).toFixed(2);
      this.wert_czk=(wert_chf/this.kurs_chf*this.kurs_czk).toFixed(2);
    }

  },

  mounted(){
    this.fetchData();
  }

};

</script>


<style>
@import './assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: var(--link-text);
  transition: 0.4s;
}

h1,h3{
  text-align: center;
}
h1{
  padding-bottom: 10px;
}
footer{
  padding: 10px;
  text-align: center;
  font-size: 12px;
  position: fixed;
  bottom: 0;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }
  input{
    font-size:20px;
  }

  #app {
    width: 500px;
    /* display: grid; */
    /* grid-template-columns: 1fr 1fr; */
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
