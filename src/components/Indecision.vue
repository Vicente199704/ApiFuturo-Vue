<template>
  <img  v-if="img" :src="img" alt="bg" />
  <div class="bg-dark"></div>

  <div class="indecision-container">
    <input v-model="question" type="text" placeholder="Hazme una pregunta" /> <!--  EL V-MODEL QUESITON HACE RELACION AL WATCH QUESTION-->
    <p>Recuerda terminar con un signo de interrogacion (?)</p>
    <div v-if="isValidQuestion"> <!--  Si isValidQuestion es TRUE, devolveremos la respuesta, de lo contrario no mostraremos ni pregunta ni respuesta. -->
      <h2>{{ question }}</h2>
      <h1>{{ answer === 'yes' ? 'Si!' : 'No!'}}</h1>
    </div>
  </div>
</template>

<script>

export default {
    data() { //En la data inicializamos las variables question y answer.
        return {
            question: null,
            answer: null,
            img: null,
            isValidQuestion: false
        }
    },
    methods:  {
        async getAnswer() {
            this.answer = 'Pensando...'
            const { answer, image } = await fetch('https://yesno.wtf/api').then( r => r.json() ) // fetch es para llamar una peticion , y fetch ()
            this.answer = answer
            this.img = image
        }
    },

    watch: {
      question(value){ //Nos muestra todo lo que ocurre en el input con un valor nuevo o un valor  antiguo console.log(value.includes('?')); //Al incluir el signo ? nos dispara un true, en caso contrario solo ocurrira un false.*/
        this.isValidQuestion = false //Sera una pregunta valida hasta que pase el if.
      if ( !value.includes('?')) return
           this.isValidQuestion = true
           this.getAnswer()
       }
    }

}

</script>

<style>
img,
.bg-dark {
  height: 100vh;
  left: 0px;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  top: 0px;
  width: 100vw;
}

.bg-dark {
  background-color: rgba(0, 0, 0, 0.4);
}

.indecision-container {
  position: relative;
  z-index: 99;
}

input {
  width: 250px;
  padding: 10px 15px;
  border-radius: 5px;
  border: none;
}
input:focus {
  outline: none;
}

p {
  color: white;
  font-size: 20px;
  margin-top: 0px;
}

h1,
h2 {
  color: white;
}

h2 {
  margin-top: 150px;
}
</style>
