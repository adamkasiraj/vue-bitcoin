<template>
    <div class="col-xs-12 col-sm-6 col-lg-4">
        <div class="panel panel-bitcoin">
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
    </div>
</template>

<script>
    import countrydata from 'country-data'

    export default {
        name: 'BitcoinCard',
        props:[ 'listing' ],
        data () {
            
        },
        filters: {
          formatMoney (value) {
            return value.toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
          },
          currencyName ( value ) {
            return countrydata.currencies[ value ].name
          },
        },
        
    }
</script>


<style lang="scss" scoped>

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
</style>
