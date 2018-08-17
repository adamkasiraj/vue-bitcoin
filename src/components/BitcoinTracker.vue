<template>
  <div class="container">

    <!-- TITLE ROW -->
    <div class="row text-center row-title">
      <div class="col-xs-12">
        <h1> Bitcoin Prices</h1>
        <p class="small timestamp"> Prices current as of {{timestamp}} </p>
        <button v-on:click="getData" class="btn btn-sm btn-primary-dark-blue text-uppercase">Refresh Data</button>
      </div>
    </div>

    <!-- FILTER/SORT ROW -->
    <div class="row row-paging hidden-xs">
      <div class="col-xs-12">
        <div class="panel panel-plain">
          <div class="panel-body">
            <div class="row">

              <!-- SORT BY CATEGORY -->
              <div class="col-xs-12 col-sm-6 col-lg-3">
                <div class="input-group input-group-sm">
                  <div class="input-group-btn">
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'currency'}"  v-on:click="alphabetizeList( 'currency' )" > Currency Denom </button>
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'last'}"  v-on:click="sortList( 'last' )" >  Last </button>
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'buy'}"  v-on:click="sortList( 'buy' )" >  Buy </button>
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'sell'}"  v-on:click="sortList( 'sell' )" >  Sell </button>
                  </div>
                </div>
              </div>

              <!-- SORT BY ORDER -->
              <div class="col-xs-12 col-sm-6 col-lg-3  col-lg-offset-6 text-right">
                <div class="input-group input-group-sm">
                  <div class="input-group-btn">
                    <button class="btn btn-default" :class="{active:selected == 'asc'}" v-on:click="reverseList( 'asc' )" > <i class="icon-arrow-up"></i>  </button>
                    <button class="btn btn-default"  :class="{active:selected == 'desc'}" v-on:click="reverseList( 'desc' )" >  <i class="icon-arrow-down"></i> </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- BITCOIN PRICING PANELS ROW -->
    <div class="row row-bitcoin">
      <div v-for="(listing) in listings" class="col-xs-12 col-sm-6 col-lg-4">
        <transition name="slide-fade" mode="out-in">
          <div class="panel panel-bitcoin" :key="listing.currency" >
            <div class="panel-heading">
              <div class="panel-title">
                <div class="row">
                  <div class="col-xs-4 text-left">
                    <p> <span class="currency"> {{ listing.currency }} </span> <span class="symbol"> ( {{ listing.info.symbol }} ) </span>  </p>
                  </div>
                  <div class="col-xs-8 text-right">
                    <p class="currency-full small"> {{ listing.currency | currencyName }} </p>
                  </div>
                </div>
              </div>

            </div>
            <div class="panel-body">
              <div class="row row-prices text-center">
                <div class="col-xs-4">
                  <p class="small"> Last </p>
                  <p class="price"> {{ listing.info.last | formatMoney }} </p>
                </div>
                <div class="col-xs-4">
                  <p class="small"> Buy </p>
                  <p class="price"> {{ listing.info.buy | formatMoney }} </p>
                </div>
                <div class="col-xs-4">
                  <p class="small"> Sell </p>
                  <p class="price"> {{ listing.info.sell | formatMoney }} </p>
                </div>
              </div>

            </div>
          </div>
        </transition>
      </div>
    </div>
      
  </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: 'HelloWorld',
        data () {
            return {
                selected: null,
                category: null,
                listings: null,
                timestamp: null
            }
        },
        mounted() {
            this.getData()       
        },
        filters: {
          formatMoney (value) {
            return value.toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
          },
          currencyName ( value ) {
            var currencies  = require('country-data').currencies
            return currencies[ value ].name
          },
        },
        methods: {
          getData () {
            this.timestamp = new Date().toLocaleDateString( 'en-US', { day: 'numeric', month: 'short', year: 'numeric', hour: 'numeric', minute: 'numeric', second: 'numeric' } );
            this.category = 'currency'
            this.selected = 'asc'
            axios
              .get('https://blockchain.info/ticker')
              .then(response => (this.convertData( response.data )) )
              .catch(error => console.log(error))
          },
          convertData( data ) {
            //convert object to array for easy sorting
            this.listings = Object.entries( data ).map(( [key, value] ) => ({ currency: key,info: value }))

            //sort list alphabetically
            this.alphabetizeList ( 'currency' )
          },
          reverseList( value ) {
            this.listings.reverse()
            this.selected = value
          },
          alphabetizeList ( value ) {
            this.category = value
            this.listings.sort(function(a, b) {
              var textA = a.currency;
              var textB = b.currency;
              return (textA > textB) ? -1 : (textA < textB) ? 1 : 0;
            })

            if( this.selected === 'desc')
              return this.listings.reverse()
            return this.listings
          },
          sortList( value ){
            this.category = value
            this.listings.sort((a, b) => a.info[value] - b.info[value])
            if( this.selected === 'desc')
              return this.listings.reverse()
            return this.listings

          }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

.row-title{
  margin-bottom:10px;
  .timestamp {
    margin-bottom: 0;
  }
}
.row-paging {
  .btn {
    margin-top: 0;
    &.active {
      cursor: not-allowed;
      pointer-events: none;
    }
  }
}
.panel-bitcoin {
  p {
    margin-bottom: 0;
  }
  .panel-heading {
    background: #003B53;
    color: #fff;
  }
  .currency {
    font-weight: bold;
    font-size: 16px;
    color: #fff;
  }
  .symbol {
    color: #fff;
  }
  .currency-full{
    color: #fff;
  }
  .price {
    font-weight:bold;
    font-size:16px;
  }

}

.slide-fade-enter-active {
  transition: all .8s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateX(10px);
  opacity: 0;
}
</style>
