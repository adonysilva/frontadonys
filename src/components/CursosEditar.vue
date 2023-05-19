<template>
    <div>
      <form @submit.prevent="submitForm">
        <div class="form-group">           
          <label for="nome" class="form-control">Nome: </label>
          <input type="text" id="nome" v-model="curso.nome" required class="form-control"/>       
        </div>
        <div class="form-group">
          <label for="cargahoraria">Carga Horária:</label>
          <input type="number" id="cargahoraria" v-model="curso.cargahoraria" required class="form-control">
        </div> 
        <div class="form-group">           
          <label for="inicio" class="form-control">Início: </label>
          <input type="date" id="inicio" v-model="curso.inicio" required class="form-control"/>       
        </div>
        <div class="form-group">           
          <label for="termino" class="form-control">Término: </label>
          <input type="date" id="termino" v-model="curso.termino" required class="form-control"/>       
        </div>
        <div class="form-group">           
          <label for="turno" class="form-control">Turno: </label>
          <input type="text" id="turno" v-model="curso.turno" required class="form-control"/>       
        </div>
        <div class="form-group">
          <label for="valor">Valor:</label>
          <input type="text" id="valor" v-model="curso.valor" required class="form-control">
        </div>
        <br>
        <button type="submit" class="btn btn-primary">Salvar</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        curso: {
          nome: '',
          cargahoraria: 0,
          inicio: '',
          termino: '',
          turno: '',
          valor: 0,
        }
      };
    },    
    created() {
      const codigocurso = this.$route.params.codigocurso;
      axios.get(`http://localhost:3000/cursos/${codigocurso}`).then(response => {
        this.curso.nome = response.data[0].nome;
        this.curso.cargahoraria = response.data[0].cargahoraria;
        this.curso.inicio = response.data[0].inicio.split('/').reverse().join('-');
        this.curso.termino = response.data[0].termino.split('/').reverse().join('-');
        this.curso.turno = response.data[0].turno;
        this.curso.valor = response.data[0].valor; 
       
        console.log(this.curso)       
      });
    },
    methods: {
      submitForm() {        
        const codigocurso = this.$route.params.codigocurso;
        this.curso.valor = this.curso.valor.replace(',', '.');        
        axios.put(`http://localhost:3000/cursos/${codigocurso}`, this.curso).then(() => {
          this.$router.push({ name: 'cursos' });
        });
      }
    }
  };
  </script>