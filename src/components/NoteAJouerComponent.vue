<template>
    <div>
        <button @click="this.play()">play</button>
        <button @click="this.stop()">Stop play</button>
        <p>Dropdown tempo</p>
        <select  v-model="this.tempo" @change="changeTempo($event)">
            <option v-for="tempot in 200" :key=tempot>{{ tempot }}</option>
        </select>
        <p>double drop down metronome</p>
        <select  v-model="this.metronomeNumerateur" @change="changeNumerateur($event)" >
            <option v-for="rythme in 16" :key=rythme >{{ rythme }}</option>
            </select>
            <select  v-model="this.metronomeDenominateur"  @change ="changeDenominateur($event)">
                <option v-for="rythme in 16" :key=rythme >{{ rythme }}</option>

            </select>

        <h1>{{ this.newNote }}</h1>
        <h5>{{ this.intervalText }}</h5>
        <h5>Score : {{ this.score }}</h5>
        <p> Niveau</p>

    </div>
</template>
<script>
import coupFaible from '/src/components/coupFaible.mp3';
import coupFort from '/src/components/coupFort.mp3';

export default{
    props: {
        //Peut etre qu'on peut definir un array de note ici
        notesSelected: { required: true, type: [Object] },
        ColotNotes : {requiered : true, type:  [Object]},
        noteTuner : {required : true,type : String},
        listeNote : {required : true, type: [Object]}

    },
    data(){
        return{
            index : 0,
            isPlaying : false,
            tempo : 30,
            metronomeNumerateur :4,
            metronomeDenominateur :4,
            lastNote : "A",
            newNote :"A",
            Interval : 3,
            score : 0,
            listeNoteTot : this.listeNote,
            intervalListe : [
                { id : 0 , nom : "R"},
                { id : 1 , nom : "b2"},
                { id : 2 , nom : "2"},
                { id : 3 , nom : "b3"},
                { id : 4 , nom : "3"},
                { id : 5 , nom : "4"},
                { id : 6 , nom : "b5"},
                { id : 7 , nom : "5"},
                { id : 9 , nom : "b6"},
                { id : 10 , nom : "6"},
                { id : 11, nom : "b7"},
                { id : 12, nom : "7"},
        ],
        intervalListeNom : [
                { id : 0 , nom : "Fondamentale"},
                { id : 1 , nom : "Seconde diminuée"},
                { id : 2 , nom : "Seconde"},
                { id : 3 , nom : "Tierce mineur"},
                { id : 4 , nom : "Tierce majeur"},
                { id : 5 , nom : "Quarte"},
                { id : 6 , nom : "Quinte diminuée"},
                { id : 7 , nom : "Quinte juste"},
                { id : 9 , nom : "Sixte mineur"},
                { id : 10 , nom : "Sixte majeur"},
                { id : 11, nom : "septième diminuée"},
                { id : 12, nom : "septième"},
        ]
        }



    },
    computed : {
            intervalText(){
                var find = this.intervalListe.find(inter => inter.id === this.Interval)
                var find2 = this.intervalListeNom.find(inter => inter.id === this.Interval)
                return find.nom+" "+find2.nom
            }
    },
    methods : {
        changeTempo(event){
            console.log(event.target)
            this.tempo =   event.target.value
            console.log("New tempo "+ this.tempo)
        },
        changeNumerateur(event){
            console.log(event.target)
            this.metronomeNumerateur =   event.target.value
            console.log("New numerateur "+ this.metronomeNumerateur)
        },
        changeDenominateur(event){
            console.log(event.target)
            this.metronomeDenominateur =   event.target.value
            console.log("New denominateru "+ this.metronomeDenominateur)
        },
        stop(){
                this.index =0
                this.isPlaying = false
                console.log("stopp")
                clearInterval(this.fct);
            },
        playSound(){
            if(this.index%this.metronomeNumerateur===0){
                const au = new Audio(coupFort );
               console.log(au)
               au.play()
            }
            else{
               const au = new Audio(coupFaible);
               console.log(au)
               au.play()
                console.log("coup faible")
            }
            //Lets go sound here

            this.index+=1
        },
        play(){
            var timeInterval = 60/this.tempo  
            //TODO -> Ajout du denominateur
            timeInterval = 4*timeInterval/this.metronomeDenominateur
            ///Calcul du temps tous les cb appeler la fct
            console.log(timeInterval)
            this.fct = setInterval(() => this.calcNewNote(), timeInterval*1000);
        },
        calcNote(){
               var find =  this.listeNoteTot.find(note => note.note === this.newNote)
               var noteExpected = (find.id + this.Interval)%12
               var noteExp = this.listeNoteTot.find(note => note.id === noteExpected)
               return noteExp.note
        },
        generateNewNote(){
            var ran = Math.floor(Math.random() * 12);
            var find = this.listeNoteTot.find(note => note.id === ran)
            return find.note
        },
        calcNewNote(){
             
            //Ajout dans une liste membre de classe ou tout simplement une variable :
            this.noteAttendue = {"note" : this.calcNote, "time" : "Comment on get le time"}

            if(this.index%this.metronomeDenominateur===0){
                // this.oldNote=this.newNote
                this.newNote = this.generateNewNote()
                this.Interval = Math.floor(Math.random() * 12);
            }
            else{
                this.Interval = Math.floor(Math.random() * 12);


            }
            console.log(this.Interval)
            this.playSound()
           

            //Lancer le calcul après 0.5 timeinterval et regarder toutes les notes qui ont ete jouée depuis 1 timetinterval de temps
            //prendre la plus proche du centre qui equivaut au coup de metronome.
            // Par rapport à une guassiene centrée au temps de metronome, definir la precision par rapport à l'endroit où on tape.

            console.log("caluculus")
            if(this.calcNote() === this.noteTuner){
                    this.score+=1
                    //Afficher un truc stympa
            }else{
                //afficher un truc pas cool
            }



        }
        //Methode that play sound


    }
}
</script>