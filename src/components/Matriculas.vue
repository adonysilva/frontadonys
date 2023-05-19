<template>
    <div class="container mt-3">
      <h2>Cadastrar nova Matrícula</h2>
      <form @submit.prevent="handleSubmit">       
        <div class="form-group">
          <label for="tipo">Curso: </label>
          <select class="form-control" id="curso" v-model="matricula.codigocurso">
            <option v-for="curso in cursos" :key="curso.codigocurso" :value="curso.codigocurso">{{ curso.nome }}</option>
          </select>
        </div>
        <div class="form-group">
          <label for="tipo">Aluno: </label>
          <select class="form-control" id="aluno" v-model="matricula.codigoaluno">
            <option v-for="aluno in alunos" :key="aluno.codigoaluno" :value="aluno.codigoaluno">{{ aluno.nome }}</option>
          </select>
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
        matricula: {
          codigoaluno: 0,
          codigocurso: 0,
        },
        cursos: [],
        alunos: [],
      };
    },
    mounted() {
      this.carregarCursos();
      this.carregarAlunos();
    },
    computed: {
    },
    methods: {
      carregarAlunos() {
        axios.get('http://localhost:3000/alunos').then((response) => {
          this.alunos = response.data;
        });
      },
      carregarCursos() {
        axios.get('http://localhost:3000/cursos').then((response) => {
          this.cursos = response.data;
        });
      },
      handleSubmit() {
        const matricula = {
          codigoaluno: this.matricula.codigoaluno,
          codigocurso: this.matricula.codigocurso
          
        };
  
        //console.log(produto);
        
         axios.post('http://localhost:3000/matriculas', matricula).then(() => {
          // Redireciona para a página de listagem de tipos de produto
          alert('Matrícula realizada com sucesso!');
          // Limpar Campos
          this.matricula.codigoaluno = 0;
          this.matricula.codigocurso = 0;
          
        });
        
      },
    },
  };
  </script>