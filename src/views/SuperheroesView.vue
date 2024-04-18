<template>
    <div class="heroes">
    <h1>Er zijn {{ filteredHeroes.length }} Superheroes</h1>

    <button @click="sortByIntelligence">Sorteer op intelligentie</button>
    <button @click="sortByStrength">Sorteer op sterkte</button>

    <input type="text" placeholder="Zoeken..." v-model="searchText">

    <br>

    <input type="radio" name="gender" value="All" v-model="gender">All
    <input type="radio" name="gender" value="Male" v-model="gender">Male
    <input type="radio" name="gender" value="Female" v-model="gender">Female

    <select v-model="size">
        <option value="xs">xs</option>
        <option value="sm">sm</option>
        <option value="md">md</option>
        <option value="lg">lg</option>
    </select>

    <br>

    <div class="hero" v-for="h in filteredHeroes">
        <header>{{ h.name }}</header>
        <p> {{ h.biography.fullName }} &nbsp</p>
        <img v-if="size == 'xs'" :src="h.images.xs">
        <img v-if="size == 'sm'" :src="h.images.sm">
        <img v-if="size == 'md'" :src="h.images.md">
        <img v-if="size == 'lg'" :src="h.images.lg">

        <p>intelligence {{ h.powerstats.intelligence }}</p>
        <p>Strength {{ h.powerstats.strength }}</p>
        <p>Speed {{ h.powerstats.speed }}</p>
        <p> {{ h.appearance.gender }}</p>
        
    </div>
</div>
</template>

<script>

    import axios from 'axios';

    export default
{
    data() {
        return {
            heroes: [],
            searchText: '',
            gender: 'All',
            size: 'sm'
        }
    },

    async mounted() {
        let response = await axios.get('https://akabab.github.io/superhero-api/api/all.json');
        this.heroes = response.data;
    },

    computed: {
        filteredHeroes()
        {
            return this.heroes.filter(h => h.name.toLowerCase().includes(this.searchText.toLowerCase())).filter(h => h.appearance.gender == this.gender || this.gender == 'All');
        }
    },
      
    methods: {
        sortByIntelligence()
        {
            this.heroes.sort((a,b) => b.powerstats.intelligence - a.powerstats.intelligence)
        },
        sortByStrength()
        {
            this.heroes.sort((a,b) => b.powerstats.strength - a.powerstats.strength)
        }

    }
}
</script>

<style>
.hero
{
    background-color: lightblue;
    /* width: 250px; */
    float: left;
    text-align: center;
    margin: 20px;
    /* height: 500px */


}

.hero header
{
    background-color: blue;
    color: white;
    font-size: 150%;
    padding: 10px;
}

h1
{
    text-align: center;
}

.heroes
{
    text-align: center;
}


</style>