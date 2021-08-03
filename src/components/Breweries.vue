<template>
    <div class="container">
        <main>
            <BreweriesSelect :breweries="breweries"  @getState="getState($event)"></BreweriesSelect>
            <section :class=" isThereData ? 'list-of-breweries' : 'no-data'">
                <p v-if="!isThereData"> Data not found, please try again with a different state</p>
                <!-- looping through breweries -->
                <div class="brewery-card" v-for="brewery in breweries" :key="brewery.id">
                    <h2>{{ brewery.name }}</h2>
                    <div class="line-break"></div>
                    <p>Located at {{ brewery.city}}</p>
                    <!-- Show if brewery's site is available -->
                    <p v-if="brewery.website_url">Check out their website 
                        <a target="_blank" :href="brewery.website_url">here!</a>
                    </p>
                    <p v-else>No website available</p>
                </div>
            </section>
        </main>
    </div>
</template>

<script>
import axios from 'axios'
import BreweriesSelect from './BreweriesSelect.vue'
export default {
    name: 'Breweries',
    components :{
        BreweriesSelect
    },
    data() {
        return {
            breweries: [],
            isThereData: true
        }   
    },
    methods : {
        getBreweries: async function (title) {
        await axios.get(`https://api.openbrewerydb.org/breweries?by_state=${title}`).then((response) => {
            if(response && response.data) {
                this.isThereData = true
                this.breweries = response.data
            } 
            // fallback if no data is returned
            if(!Object.keys(response.data).length){
                this.isThereData = false
            } 
        })
        },
        getState(title) {
            //recivieing data from child component through payload (title), 
            // calling getBreweries and passing value for api call
            this.getBreweries(title)
        }
    }
}
</script>

<style lang="scss">
    .container{
        max-width: 1400px;
        margin: 0 auto;
        main{
            padding-bottom: 40px;
        }
    }

    .no-data {
        p {
            font-size: 18px;
            padding: 20px 10px 0;
        }
    }

    .list-of-breweries {
        display: grid;
        display: grid;
        grid-template-columns: repeat(3, minmax(10rem, 1fr));
        gap: 40px;
        padding-top: 40px;
        .brewery-card {
            text-align: left;
            padding: 10px 30px;
            border: 2px solid #011f4b;
            border-radius: 9px;
            height: 180px;
            overflow-y: scroll;
            h2 {
                margin-bottom: 0;
                color: burlywood;
            }
            .line-break {
                border: 1px solid #011f4b;
                width: 100px;
                margin-top: 0px;
                opacity: 0.8;
            }
            a {
                color: black;
            }
        }
    }

    @media screen and (max-width: 1420px) {
        .list-of-breweries {
            grid-template-columns: repeat(2, minmax(10rem, 1fr));
            padding: 40px;
        }
    }


    @media screen and (max-width: 700px) {
        .list-of-breweries {
            grid-template-columns: repeat(1, minmax(10rem, 1fr));
            padding: 20px;
        }
    }

</style>