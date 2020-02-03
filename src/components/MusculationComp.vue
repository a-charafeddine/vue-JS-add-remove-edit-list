<template>
    <div>
        <h1>List of Articles</h1>
        <ul class="list-group">
            <li class="text-left list-group-item" 
                v-bind:key="musculation.id" 
                v-for='musculation in MusculationJSON'>
                    <ListOfArtComp 
                        v-bind:musculationProps = "musculation" 
                        v-on:deleteArt="deletThisArt"
                        v-on:updateArt="updateThisArt"
                    />
            </li>
        </ul>
        <form class="mt-3">
            <div class="form-group">
                <input 
                    v-model="articleNameModel" 
                    type="text" 
                    class="form-control" 
                    placeholder="Name of the articles">
                <input 
                    v-model="learndeCheck" 
                    type="checkbox" 
                    name="Learned" 
                    id="learned">
            </div>

            <!--Button de modification du nouveau Articles--> 
            <button 
                v-if="thisArt" 
                v-on:click="updateArticle"  
                class="btn btn-block btn-warning ">
                    Update Article
            </button>
            
            <!--Button d'ajout du nouveau Articles-->
            <button 
                v-else 
                v-on:click="addArticle" 
                class="btn btn-block btn-success">
                    Add Article
            </button>
        </form>
    </div>
</template>

<script>
import axios from 'axios'
import ListOfArtComp from './ListOfArtComp'
export default {
    name : "muscu",
    components :{
        ListOfArtComp
    },
    data(){
        return {
            MusculationJSON:[],
            articleNameModel:'',
            learndeCheck:false,
            thisArt:null
        }
    },
    methods: {
        /*----Récuperation du Data à partir du fichier JSON----*/
        getArtMusc(){
            axios.get('http://localhost:3000/musculationJS')
                 .then(res => this.MusculationJSON = res.data)
        },
        /*----Adjout d'un nouveau Article----*/
        addArticle(e){
            e.preventDefault();
            let newArt = {
                articleName: this.articleNameModel,
                learned: this.learndeCheck
            }
            if(this.articleNameModel != ''){
                axios.post('http://localhost:3000/musculationJS', newArt)
                     .then(res => {
                         this.MusculationJSON = [res.data, ...this.MusculationJSON],
                         this.articleNameModel = '',
                         this.learndeCheck = false
                     })
            }else{
                alert('Remplissez le champ');
            }
        },
        /*----Suppression d'un Article----*/
        deletThisArt(id){
            axios.delete('http://localhost:3000/musculationJS/' + id)
                 .then(() => {
                        this.MusculationJSON =  this.MusculationJSON.filter(musculation => { return musculation.id !== id})
                    }
                 )
        },
        /*----Remplir les champs avec les données d'un Article----*/
        updateThisArt(art){
            this.thisArt = art;
            this.articleNameModel = art.articleName;
            this.learndeCheck = art.learned;
        },
        /*----Modification d'un Article----*/
        updateArticle(){
            let articleToUpdate={
                ...this.thisArt,
                articleName: this.articleNameModel,
                learned:this.learndeCheck
            }
            axios.put('http://localhost:3000/musculationJS/'+ articleToUpdate.id, articleToUpdate)
                 .then(res => {
                     this.MusculationJSON = this.MusculationJSON.map(t=>{
                         if(res.data.id === t.id){
                             return res.data
                         }
                         return t
                     }),
                     this.articleNameModel='',
                     this.learndeCheck='',
                     this.thisArt=null
                 })
        }
    },
    created(){
        /*----Lancement de la method gerArtMusc qui affiche tout les articles sur le fichier JSON----*/
        this.getArtMusc()
    }
}
</script>

<style>

</style>