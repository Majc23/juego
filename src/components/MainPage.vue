<template>
  <div class="hello">
    <div>
      <form>
        <b-row>
          <b-col sm="10">
            <h1>{{ msg }}</h1>
            
          </b-col>
          <b-col v-if="ganadores.length > 0 " sm="2">
            <b-button @click="changeban">records</b-button>
          </b-col>
        </b-row>
      </form>
    </div>
    <div class="h3">
      <form>
        <b-row>
          <b-col sm="2">
             <b-button @click="jugar">Jugar</b-button>
          </b-col>
          <b-col sm="5">
            <b-form-input type="text" v-model="usuario" placeholder="Ingrese su usuario"></b-form-input>
          </b-col>
          <b-col sm="2" v-if="usuario.length != 0">
            <p>Premio:{{premio}}</p>
          </b-col>
        </b-row>
      </form>
    </div>
    <div v-if="usuario.length != 0">
      <div v-if="pregunta.length > 0">
          <b-button v-if="this.premio > 0 " @click="retirarse">Retirarse</b-button>
          <p>Pregunta</p>
          <b-form-group :label="pregunta[0].cuerpo">
            <b-form-radio-group
              v-model="selected"
              :options="pregunta[0].opciones"
              @change="contestar"
            ></b-form-radio-group>
          </b-form-group>
      </div>
    </div>
    <div v-if="this.cont == 6 && this.usuario.length > 0 && this.banreg == false">
      <h1>Felicidades</h1>
    </div>
    <div v-if="this.ban == true && this.usuario.length > 0 && this.banreg == false">
      <h1>Fin del Juego</h1>
    </div>
    <div v-if="this.banreg == true">
      <ul>
        <b-row>
            <li v-for="(ganador,index) in this.ganadoresord" :key = "index">
                <b-col>
                  <h2>{{ganador.usuario}} - {{ganador.premio}} puntos</h2>
                </b-col>
            </li>
        </b-row>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  name: 'MainPage',
  props: {
    msg: String
  },
  data(){
    return{
      usuario:'',
      cont: 1,
      premio: 0,
      min: null,
      max: null,
      pos: null,
      ban: false,
      banreg: false,
      selected:null,
      pregunta:[],
      preguntas: [{
                 ronda: 1,
                 cuerpo: 'Es el estado más grande de México',
                 opciones: [
                   { value: 1, text: 'Puebla' },
                   { value: 2, text: 'Guadalajara' },
                   { value: 3, text: 'Chihuahua' },
                   { value: 4, text: 'Baja California Sur' }
                 ],
                 is: 3
             },
             {
                 ronda: 1,
                 cuerpo: '¿Cual es el contienente mas grande?',
                 opciones: [
                   { value: 1, text: 'America' },
                   { value: 2, text: 'Europa' },
                   { value: 3, text: 'Africa' },
                   { value: 4, text: 'Asia' }
                 ],
                 is: 4
             },
             {
                 ronda: 1,
                 cuerpo: '¿Cual es el pais mas grande territorialmente en America?',
                 opciones: [
                   { value: 1, text: 'Brasil' },
                   { value: 2, text: 'Canada' },
                   { value: 3, text: 'USA' },
                   { value: 4, text: 'Argentina' }
                 ],
                 is: 2
             },
             {
                 ronda: 1,
                 cuerpo: '¿Cual es el area metropolitana màs poblada en America?',
                 opciones: [
                   { value: 1, text: 'Los Ángeles-Long Beach-Santa Ana' },
                   { value: 2, text: 'Región metropolitana de São Paulo' },
                   { value: 3, text: 'Nueva York-Nueva Jersey-Connecticut-Pensilvania' },
                   { value: 4, text: 'Valle de México' }
                 ],
                 is: 4
             },
             {
                 ronda: 1,
                 cuerpo: ' A cuántas unidades equivale 10 decenas de millar.',
                 opciones: [
                   { value: 1, text: '1000000 unidades' },
                   { value: 2, text: '100000 unidades' },
                   { value: 3, text: '10000 unidades' },
                   { value: 4, text: '1000 unidades' }
                 ],
                 is: 2
             },
             {
                 ronda: 2,
                 cuerpo: 'Aproxima el número 58 a la decena',
                 opciones: [
                   { value: 1, text: '55' },
                   { value: 2, text: '60' },
                   { value: 3, text: '58' },
                   { value: 4, text: '50' }
                 ],
                 is: 2
             },
             {
                 ronda: 2,
                 cuerpo: 'Qué cantidad expresa el número romano V',
                 opciones: [
                   { value: 1, text: '20' },
                   { value: 2, text: '1' },
                   { value: 3, text: '5' },
                   { value: 4, text: '10' }
                 ],
                 is: 3
             },
             {
                 ronda: 2,
                 cuerpo: 'Qué número resulta si divides 56 entre 7',
                 opciones: [
                   { value: 1, text: '6' },
                   { value: 2, text: '9' },
                   { value: 3, text: '8' },
                   { value: 4, text: '7' }
                 ],
                 is: 3
             },
             {
                 ronda: 2,
                 cuerpo: 'Cuál es la representación gráfica del número nueve mil treinta y seis',
                 opciones: [
                   { value: 1, text: '90036' },
                   { value: 2, text: '936' },
                   { value: 3, text: '9136' },
                   { value: 4, text: '9036' }
                 ],
                 is: 4
             },
             {
                 ronda: 2,
                 cuerpo: 'Escribe el número ordinal trigésimo quinto en cifras',
                 opciones: [
                   { value: 1, text: '135' },
                   { value: 2, text: '35' },
                   { value: 3, text: '25' },
                   { value: 4, text: '15' }
                 ],
                 is: 2
             }
             ,
             {
                 ronda: 3,
                 cuerpo: 'Qué hora es si, tanto la manilla pequeña del reloj como la grande están en las 3',
                 opciones: [
                   { value: 1, text: 'Las tres y tres' },
                   { value: 2, text: 'Las tres  y media' },
                   { value: 3, text: 'Las tres y cuarto' },
                   { value: 4, text: 'Las tres en punto' }
                 ],
                 is: 3
             },
             {
                 ronda: 3,
                 cuerpo: 'Cálculo mental rápido: cuánto es 140 entre 10',
                 opciones: [
                   { value: 1, text: '14' },
                   { value: 2, text: '1400' },
                   { value: 3, text: '40' },
                   { value: 4, text: '1.4' }
                 ],
                 is: 1
             },
             {
                 ronda: 3,
                 cuerpo: 'Cuánto es un lustro',
                 opciones: [
                   { value: 1, text: '1 año' },
                   { value: 2, text: '10 años' },
                   { value: 3, text: '20 años' },
                   { value: 4, text: '5 años' }
                 ],
                 is: 4
             },
             {
                 ronda: 3,
                 cuerpo: 'Son un conjunto de membranas que protegen al sistema nervioso',
                 opciones: [
                   { value: 1, text: 'Tejidos' },
                   { value: 2, text: 'Neuronas' },
                   { value: 3, text: 'Meninges' },
                   { value: 4, text: 'Liquido Cefalo raquideo' }
                 ],
                 is: 3
             },{
                 ronda: 3,
                 cuerpo: 'A cuánto equivale π',
                 opciones: [
                   { value: 1, text: '3.141592' },
                   { value: 2, text: '3.149215' },
                   { value: 3, text: '3.144423' },
                   { value: 4, text: '3.144123' }
                 ],
                 is: 1
             },
             {
                 ronda: 4,
                 cuerpo: '7/5 + 2/3 - 1 =',
                 opciones: [
                   { value: 1, text: '17/15' },
                   { value: 2, text: '8/4' },
                   { value: 3, text: '1' },
                   { value: 4, text: '16/15' }
                 ],
                 is: 4
             },
             {
                 ronda: 4,
                 cuerpo: '¿Qué formula es esta? S = π x R² ',
                 opciones: [
                   { value: 1, text: 'Diametro de un circulo' },
                   { value: 2, text: 'Superficie de un circulo' },
                   { value: 3, text: 'Volumen de un cilindro' },
                   { value: 4, text: 'Superficie de paralelogramos' }
                 ],
                 is: 2
             },
             {
                 ronda: 4,
                 cuerpo: '¿Qué expresa esta formula? e = mc²',
                 opciones: [
                   { value: 1, text: 'Circunferencia de un circulo' },
                   { value: 2, text: 'La teoria de la probablilidad' },
                   { value: 3, text: 'Equivalencia entre masa y energía' },
                   { value: 4, text: 'Volumen de un cubo' }
                 ],
                 is: 3
             },
             {
                 ronda: 4,
                 cuerpo: 'Una rueda de un coche da 4590 vueltas en 9 minutos. ¿Cuántas vueltas dará en 24 horas y 24 minutos?',
                 opciones: [
                   { value: 1, text: '2868' },
                   { value: 2, text: '746,640' },
                   { value: 3, text: '280,005' },
                   { value: 4, text: '3,343,467' }
                 ],
                 is: 2
             },{
                 ronda: 4,
                 cuerpo: '(-3)³ + (-2)³ - (-3)³ + (-1)³ =',
                 opciones: [
                   { value: 1, text: '-10' },
                   { value: 2, text: '10' },
                   { value: 3, text: '9' },
                   { value: 4, text: '-9' }
                 ],
                 is: 4
             },  
             {
                 ronda: 5,
                 cuerpo: '¿Cual es el valor aproximado del número áureo o número de oro?',
                 opciones: [
                   { value: 1, text: '1.681305' },
                   { value: 2, text: '1.618033' },
                   { value: 3, text: '1.611903' },
                   { value: 4, text: '1.614412' }
                 ],
                 is: 2
             },{
                 ronda: 5,
                 cuerpo: 'Juan tiene 20 años menos que su padre y este tiene el triple de los años de su hijo. ¿Qué edad tienen cada uno?',
                 opciones: [
                   { value: 1, text: '10 juan, 30 el padre' },
                   { value: 2, text: '11 juan, 31 el padre' },
                   { value: 3, text: '12 juan, 32 el padre' },
                   { value: 4, text: '13 juan, 33 el padre' }
                 ],
                 is: 1
             },{
                 ronda: 5,
                 cuerpo: '¿Cuánto es la cuarta parte de la tercera parte?',
                 opciones: [
                   { value: 1, text: 'Un doceavo' },
                   { value: 2, text: 'Un séptimo' },
                   { value: 3, text: '3/4 partes' },
                   { value: 4, text: '4/6 partes' }
                 ],
                 is: 1
             },{
                 ronda: 5,
                 cuerpo: '¿Cuánto habrá que rebajar un producto para que valga lo mismo que valía antes de que incrementase un 25% su precio?',
                 opciones: [
                   { value: 1, text: 'Un 25%' },
                   { value: 2, text: 'Un 50%' },
                   { value: 3, text: 'Un 20%' },
                   { value: 4, text: 'Un 10%' }
                 ],
                 is: 3
             },{
                 ronda: 5,
                 cuerpo: 'Seis personas pueden vivir en un hotel durante 12 días por 792 euros. ¿Cuánto costará el hotel de 15 personas durante 8 días?',
                 opciones: [
                   { value: 1, text: 'Si se quedan 15 personas durante 8 dias les saldra 1320 euros' },
                   { value: 2, text: 'Si se quedan 15 personas durante 8 dias les saldra 1250 euros' },
                   { value: 3, text: 'Si se quedan 15 personas durante 8 dias les saldra 1105 euros' },
                   { value: 4, text: 'Si se quedan 15 personas durante 8 dias les saldra 1505 euros' }
                 ],
                 is: 1
             }
        ],
      ganadores:[],
      ganadoresord:[]
    }
  },
  methods: {
    contestar(){
        if(this.selected == this.pregunta[0].is){
          this.premio += 100;
          this.cont += 1;
          this.selected = null;
          this.pregunta.splice(0,1);
          if(this.cont == 6 ){
            this.ganadores.push({
                usuario: this.usuario,
                premio: this.premio
              });
            
              this.ganadoresord = this.ganadores.sort((a,b) => {
              if(a.premio < b.premio){
                return -1;
              }
              if(a.premio > b.premio){
                return 1;
              }
              return 0;
            }).reverse();
          }
        }
        else{
          this.ban = true;
          this.pregunta.splice(0,1);
          this.cont = 1;
          //this.usuario = '';
          this.premio = 0;
        }
    },
    jugar(){
    
      this.ban = false;
      this.usuario = '';

      if(this.cont ==6){
        this.cont = 1;
        this.premio = 0;
      }
      if(this.banreg == true){
        this.banreg = false;
      }
    },
    changeban(){
      if(this.banreg == true){
        this.banreg = false;
      }else{
        this.banreg = true;
      }
    },
    retirarse(){
          this.ganadores.push({
                usuario: this.usuario,
                premio: this.premio
              });
          
              this.ganadoresord = this.ganadores.sort((a,b) => {
              if(a.premio < b.premio){
                return -1;
              }
              if(a.premio > b.premio){
                return 1;
              }
              return 0;
            }).reverse();
            
          this.ban = true;
          this.pregunta.splice(0,1);
          this.cont = 1;
          this.premio = 0;
    }
  },
  updated: function () {
        if(this.usuario.length > 0 && this.pregunta.length < 1 && this.ban == false){
          
          switch (this.cont) {
            case 1:
              this.min = Math.ceil(0);
              this.max = Math.floor(4);
              this.pos = Math.floor(Math.random() * (1 + this.max - this.min) + this.min);
              this.pregunta.push(this.preguntas[this.pos]);
              break;
            case 2:
              this.min = Math.ceil(5);
              this.max = Math.floor(9);
              this.pos = Math.floor(Math.random() * (1 + this.max - this.min) + this.min);
              this.pregunta.push(this.preguntas[this.pos]);
              break;  
            case 3:
              this.min = Math.ceil(10);
              this.max = Math.floor(14);
              this.pos = Math.floor(Math.random() * (1 + this.max - this.min) + this.min);
              this.pregunta.push(this.preguntas[this.pos]);
              break; 
            case 4:
              this.min = Math.ceil(15);
              this.max = Math.floor(19);
              this.pos = Math.floor(Math.random() * (1 + this.max - this.min) + this.min);
              this.pregunta.push(this.preguntas[this.pos]);
              break; 
            case 5:
              this.min = Math.ceil(20);
              this.max = Math.floor(24);
              this.pos = Math.floor(Math.random() * (1 + this.max - this.min) + this.min);
              this.pregunta.push(this.preguntas[this.pos]);
              break;
              
            default:
              break;
          }
        }
    }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
