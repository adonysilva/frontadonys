<template>
    <div class="container mt-3">
      <h2>Cadastrar Curso</h2>
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="nome">Nome:</label>
          <input type="text" class="form-control" id="nome" v-model="curso.nome" required>
        </div>        
        <div class="form-group">
          <label for="cargahoraria">Carga Horária:</label>
          <input type="number" id="cargahoraria" v-model="curso.cargahoraria" required class="form-control">
        </div>  
        <div class="form-group">
          <label for="inicio">Início:</label>
          <input type="date" id="inicio" v-model="curso.inicio" required class="form-control">
        </div> 
        <div class="form-group">
          <label for="termino">Término:</label>
          <input type="date" id="termino" v-model="curso.termino" required class="form-control">
        </div> 
        <div class="form-group">
          <label for="turno">Turno:</label>
          <select id="turno" v-model="curso.turno" required class="form-control">
            <option value="">Selecione um turno</option>
            <option value="matutino">Matutino</option>
            <option value="vespertino">Vespertino</option>
            <option value="noturno">Noturno</option>
          </select>
        </div>  
        <div class="form-group">
          <label for="Valor">Valor:</label>
          <input type="number" id="valor" v-model="curso.valor" required class="form-control">
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
          cargahoraria: '',
          termino: '',
          inicio: '',
          turno: '',
          varlor: 0

        },
      };
    },
    mounted() {
    },
    computed: {      
    },
    methods: { 
           
      handleSubmit() {
        const curso = {
          nome: this.curso.nome,
          cargahoraria: this.curso.cargahoraria,
          inicio: this.curso.inicio,
          termino: this.curso.termino,
          turno: this.curso.turno,
          valor: this.curso.valor,
        };
        console.log(curso);  
         axios.post('http://localhost:3000/cursos', curso).then(() => {
          // Redireciona para a página de listagem de alunos
          this.$router.push({ name: 'cursos' });
        });
        
      },
    },
  };
  </script>
  