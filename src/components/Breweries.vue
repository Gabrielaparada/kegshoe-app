<template>
    <div class="container">
        <main>
            <div class="brewery-select">
                <label for="breweries">Choose a State:</label>
                <select v-model="selectedState" id="breweries" name="cars">
                    <option disabled selected value>Select option</option>
                    <option v-for="(state, index) in states" :value="state" :key="index"> {{ state }}</option>
                </select>
                <!-- disable button if a state hasn't been selected -->
                <button :disabled="!isDisabled" @click="getBreweries()">Show me breweries!</button>
                <!-- show if brewery hasn't been selected -->
                <p v-if="breweries.length === 0">Use dropdown to filter breweries by state.</p>
            </div>
            <div v-if="breweries" class="list-of-breweries">
                <!-- looping through breweries -->
                <div class="brewery-card" v-for="brewery in breweries" :key="brewery.id">
                    <h2>{{ brewery.name }}</h2>
                    <div class="line-break"></div>
                    <p>Located at {{ brewery.city}}</p>
                    <!-- Show only if brewery's website is available -->
                    <p v-if="brewery.website_url">Check it out at 
                        <a target="_blank" :href="brewery.website_url">here!</a>
                    </p>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Breweries',
    data() {
        return {
            breweries: [],
            states: [ 'Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Colorado', 'Connecticut',
                    'Delaware', 'Florida', 'Georgia', 'Hawaii', 'Idaho', 'Illinois', 'Indiana', 'Iowa', 'Kansas',
                    'Kentucky', 'Louisiana', 'Maine', 'Maryland', 'Massachusetts', 'Michigan', 'Minnesota', 
                    'Mississippi', 'Missouri', 'Montana', 'Nebraska', 'Nevada', 'New Hampshire', 'New Jersey', 
                    'New Mexico', 'New York', 'North Carolina', 'North Dakota', 'Ohio', 'Oklahoma', 'Oregon',
                    'Pennsylvania', 'Rhode Island', 'South Carolina', 'South Dakota', 'Tennessee', 'Texas', 'Utah', 
                    'Vermont', 'Virginia', 'Washington', 'West Virginia', 'Wisconsin', 'Wyoming', 'WERE HERE!'],
            selectedState: ''
        }   
    },
    computed: {
        isDisabled() {
        return this.selectedState !== ''
        }
    },
    methods : {
        getBreweries: async function () {
        await axios.get(`https://api.openbrewerydb.org/breweries?by_state=${this.selectedState}`).then((response) => {
            if(response && response.data) {
                this.breweries = response.data
            }
        })
        }
    }
}
</script>

<style lang="scss">
    .list-of-breweries {
        display: grid;
        display: grid;
        grid-template-columns: repeat(3, minmax(10rem, 1fr));
        gap: 40px;
        .brewery-card {
            text-align: left;
            padding: 10px 30px;
            border: 2px solid #011f4b;
            border-radius: 9px;
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

    .container{
        max-width: 1400px;
        margin: 0 auto;
        main{
            padding-bottom: 40px;
        }
    }

    .brewery-select {
        margin-bottom: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        button {
            background-color: #005b96;
            border: none;
            color: white;
            font-weight: 600;
            border-radius: 3px;
            padding: 8px 10px;
            margin-top: 10px;
            cursor: pointer;
            font-size: 16px;
            &:disabled{
                border: 1px solid #999999;
                background-color: #cccccc;
                color: #666666;
                pointer-events: auto;
                cursor: not-allowed;
            }
        }
        select{
            width: 200px;
            padding: 5px 10px;
            border-radius: 5px;
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