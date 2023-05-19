<template>
    <div class="container mt-3">
      <h2>Cadastrar Aluno</h2>
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="nome">Nome: </label>
          <input type="text" class="form-control" id="nome" v-model="aluno.nome" required>
        </div>
        <div class="form-group">
          <label for="endereco">Endereço: </label>
          <textarea class="form-control" id="endereco" v-model="aluno.endereco"></textarea>
        </div>
        <div class="form-group">
          <label for="telefone">Telefone: </label>
          <input type="text" id="quantidade" v-model="aluno.telefone" required class="form-control">
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
        aluno: {
          nome: '',
          endereco: '',
          telefone: '',
        },
      };
    },
    mounted() {
    },
    computed: {      
    },
    methods: {      
      handleSubmit() {
        const aluno = {
          nome: this.aluno.nome,
          telefone: this.aluno.telefone,
          endereco: this.aluno.endereco,
        };
         axios.post('http://localhost:3000/alunos', aluno).then(() => {
          // Redireciona para a página de listagem de alunos
          this.$router.push({ name: 'alunos' });
        });
        
      },
    },
  };
  </script>
  