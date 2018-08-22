<template>
    <div class="card card-bitcoin">
        <div class="card-title d-flex">
            <div class="mr-auto text-left">
                <p> <span class="currency"> {{ listing.currency }} </span> <span class="symbol"> ( {{ listing.info.symbol }} ) </span>  </p>
            </div>
            <div class="ml-auto text-right">
                <p class="currency-full small"> {{ listing.currency | currencyName }} </p>
            </div>
        </div>
        <div class="card-body d-flex justify-content-center">
            <div class="price-listing text-center">
                <p class="small"> Last </p>
                <p class="price"> {{ listing.info.last | formatMoney }} </p>
            </div>
            <div class="price-listing text-center">
                <p class="small"> Buy </p>
                <p class="price"> {{ listing.info.buy | formatMoney }} </p>
            </div>
            <div class="price-listing text-center">
                <p class="small"> Sell </p>
                <p class="price"> {{ listing.info.sell | formatMoney }} </p>
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
.card-bitcoin{
    width:100%;
    @media (min-width: 576px) {
        width: 49%;
    }
    @media (min-width: 992px) {
        width: 32%;
    }
    margin-bottom:10px;

    p {
        margin-bottom:0;
    }

    .card-title {
        padding: 10px;
        background: #003B53;
        color: #fff;

        .currency-full{
            color: #fff;
        }

    }
    .price-listing {
        padding: 5px 10px;
        .price {
            font-weight:bold;
            font-size:16px;
        }
    }
}

//FLEXBOX UTIL CLASSES
.d-flex {
    display: -webkit-box;
    display: -moz-box;
    display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;
}
.mr-auto {
    margin-right: auto;
}
.justify-content-center {
    -webkit-box-pack: distribute;
    justify-content: space-around;
}

</style>
