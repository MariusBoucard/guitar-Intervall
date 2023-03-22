<template>
    <div>
        <h1>Play :</h1>
        <p>Playbutton</p>
        <h2>Stop play</h2>
        <p>Dropdown tempo</p>
        <p>double drop down metronome</p>
        <h1>{{ this.newNote }}</h1>
        <h5>{{ this.intervalText }}</h5>
        <h5>Score</h5>
        <p> Niveau</p>

    </div>
</template>
<script>
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
            tempo : 100,
            metronomeNumerateur :4,
            metronomeDenominateur :4,
            lastNote : "A",
            newNote :"A",
            lastInterval : "3",
            newInterval : "3",
            score : 0,
            listeNoteTot : this.listeNote
        }



    },
    computed : {
            intervalText(){
                return this.interval+"caca"
            }
    },
    method : {
        stop(){
                this.index =0
                this.isPlaying = false
                clearInterval(this.fct);
            },
        playSound(){
            new Audio("https://audio.code.org/win3.mp3").play();
            //Lets go sound here

            this.index+=1
        },
        play(){
            var timeInterval = 1/this.tempo 
            //TODO -> Ajout du denominateur
            timeInterval = timeInterval/this.metronomeDenominateur
            ///Calcul du temps tous les cb appeler la fct
            this.fct = setInterval(this.newNote(), timeInterval);
            console.log(this.fct)
        },
        calcNote(){
               var find =  this.listeNoteTot.find(note => note.note === this.newNote)
               var noteExpected = (find.id + this.interval)%12
               var noteExp = this.listeNoteTot.find(note => note.id === noteExpected)
               return noteExp.note
        },
        generateNewNote(){
            var ran = Math.floor(Math.random() * 12);
            var find = this.listeNoteTot.find(note => note.id === ran)
            return find.note
        },
        newNote(){
            if(this.calcNote() === this.noteTuner){
                    this.score+=1
                    //Afficher un truc stympa
            }else{
                //afficher un truc pas cool
            }
            if(this.index%this.metronomeDenominateur===0){
                // this.oldNote=this.newNote
                this.newNote = this.generateNewNote()
                this.interval = Math.floor(Math.random() * 12);
            }
            this.playSound()



        }
        //Methode that play sound


    }
}
</script>