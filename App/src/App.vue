<template>
    <div id="app">

        <app-header :changeSearch= "changeSearch"/>

        <div class="container">
            <h1 class="pt-3 pb-3">Персонажи Marvel</h1>


            <!-- <pre>{{characters}}</pre> 
            передаем API - application computer interface  интерполяция

             <pre>characterIndex: {{characterIndex}}</pre>
             <pre>search: {{search}}</pre>
             <pre>character.name.toLowerCase().indexOf(search.toLowerCase()): {{character.name.toLowerCase().indexOf(search.toLowerCase())}}</pre> 
            -->
            




            <app-modal :character="character"  />

            <spinner v-if="loading"/>

            <div class="row">

                <h5 v-if="!searchCharacters.length&&!loading">Немного жаль, но персонажа с таким именем пока нет во вселенной.</h5>

                      <!-- сюда должна передаваться инфа из API автоматически, то есть нужно не хардкод, а свойства элементов передавать -->
                     <!-- :перед src значит node.js будет воспринимать как  -->
                <div 
                    v-for="(element, index) in searchCharacters"
                    :key="element.id"
                    class="card mb-3 col-sm-12 col-md-6 col-lg-4">

                    <div class="row g-0">
                        <div class="col-md-4">
                        
                            <img :src="element.thumbnail" 
                                :alt="element.name"
                                style= "max-width: 100%" 
                                class="img-fluid rounded-start;"
                            >

                        </div>
                        
                        <div class="col-md-8">
                            <div class="card-body">
                                <h5 class="card-title">{{element.name}}</h5>
                                               
                                <!-- Button trigger modal -->
                                <button type="button" 
                                    class="btn btn-secondary" 
                                    data-bs-toggle="modal" 
                                    data-bs-target="#exampleModal"
                                    @click="characterIndex = index">
                                    Подробнее
                                </button>
                
            
                            </div>
                        </div>
                    </div>
                </div>       
            </div>
        </div>

    </div>
</template>

<script>
    import Spinner from "./components/Spinner";
    import AppModal from "./components/AppModal";
    import AppHeader from "./components/AppHeader";

    export default {
        name: 'App',
        components: {
            AppHeader,
            AppModal,
            Spinner,
        },
        data() {
            return {
                loading: false,
                characters: [],
                characterIndex: 0,
                search: "",     
            }
        },
        methods: {
            fetchCharacters: function() {
                return fetch("https://netology-api-marvel.herokuapp.com/characters")
                // передали входные данные respond в формат json
                .then(res => res.json())
                //присваиваем входным данным массив персонажей
                .then(json => this.characters=json);
            },

            changeSearch: function(value) 
            {
                this.search=value
            }
        },
        computed: {
        // вызываем функцию загрузки вместе с загрузкой страницы, пищем здесь вычисляемые функции
            character: function (){
                return this.searchCharacters[this.characterIndex] || null; //вернет значение или пусто
            },

            searchCharacters: function(){
                const {characters, search} = this
                return characters.filter((character)=>{
                    return character.name.toLowerCase().
                        indexOf(search.toLowerCase()) !== -1; //indexof ищет позицию вхождения искомого слова, и если не находит выводит -1.
                
                })

            }
        },
                
        async mounted(){ //методу тоже нужно сказать, чтоб он подождал
            this.loading=true;
            await this.fetchCharacters();//асинхронно сам загружается, не остановливая процесс, зачит надо подождпть его загрузки вруную await
            this.loading=false;
        }
    }
</script>

<style>

</style>



