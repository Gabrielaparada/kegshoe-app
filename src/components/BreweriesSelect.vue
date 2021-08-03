<template>
    <section class="brewery-select">
        <div class="inner-container">
            <label for="breweries">Choose a State:</label>
            <select v-model="selectedState" id="breweries" name="cars">
                <option disabled selected value>Select option</option>
                <option v-for="(state, index) in states" :value="state" :key="index"> {{ state }}</option>
            </select>
        </div>
        <!-- disable button if a state hasn't been selected -->
        <button :disabled="!isDisabled" @click="getList">Show me breweries!</button>
    </section>
</template>

<script>
export default {
    name: 'BreweriesSelect',
    props: [ 'breweries'],
    data() {
        return {
            isReady: false,
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
        getList () {
            // sending data to parent component to filter by state
            this.$emit('getState', this.selectedState)
            this.isReady = true
        }
    } 
}
</script>

<style lang="scss">
    .brewery-select {
            display: flex;
            justify-content: center;
            align-items: center;
        label {
            padding-right: 8px;
        }
        button {
            background-color: #005b96;
            border: none;
            color: white;
            font-weight: 600;
            border-radius: 3px;
            cursor: pointer;
            margin-left: 8px;
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
            padding: 8px 10px;
            border-radius: 5px;
        }
    }

    @media screen and (max-width: 700px) {
        .brewery-select {
            flex-direction: column;
            button {
                margin-top: 15px;
            }
        }
    }

</style>