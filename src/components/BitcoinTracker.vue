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
                  <label class="form-control"> Sort: </label> 
                  <div class="input-group-btn">              
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'currency'}"  v-on:click="alphabetizeList( 'currency' )" > Currency Name </button>
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'last'}"  v-on:click="sortList( 'last' )" >  Last </button>
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'buy'}"  v-on:click="sortList( 'buy' )" >  Buy </button>
                    <button class="btn btn-default text-uppercase" :class="{active:category == 'sell'}"  v-on:click="sortList( 'sell' )" >  Sell </button>
                  </div>
                </div>
              </div>

              <!-- SORT BY ORDER -->
              <div class="col-xs-12 col-sm-6 col-lg-3  col-lg-offset-6 text-right">
                <div class="input-group input-group-sm">
                  <label class="form-control"> Order: </label> 
                  <div class="input-group-btn">
                    <button class="btn btn-default" :class="{active:selected == 'asc'}" v-on:click="reverseList( 'asc' )" > ASC <i class="icon-arrow-up"></i>  </button>
                    <button class="btn btn-default"  :class="{active:selected == 'desc'}" v-on:click="reverseList( 'desc' )" > DESC  <i class="icon-arrow-down"></i> </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- BITCOIN PRICING PANELS ROW -->
    <div class="row-bitcoin">
      <BitcoinCard
        v-for="listing in listings"
        v-bind:key="listing.currency" 
        v-bind:listing="listing"
      ></BitcoinCard>
    </div>
      
  </div>
</template>

<script>
    import axios from 'axios'
    import BitcoinCard from '@/components/BitcoinCard'

    export default {
        name: 'BitcoinTracker',
        components: {
          BitcoinCard
        },
        data () {
            return {
                selected: null,
                category: null,
                listings: null,
                timestamp: null
            }
        },
        props: [ 'listing' ],
        mounted() {
            this.getData()       
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
  label {
    border:none;
    z-index:0;
  }
  .btn {
    margin-top: 0;
    &.active {
      cursor: not-allowed;
      pointer-events: none;
    }
  }
}

.row-bitcoin {
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  
  -webkit-flex-flow: row wrap;
  flex-flow: row wrap;
  justify-content: space-between;
}

</style>
