<template>
    <div>
      <h1>{{ curso.nome }}</h1>
      <p>Inicio: {{ curso.inicio }}</p>
      <p>Término: {{ curso.termino }}</p>
      <p>Turno: {{ curso.turno }}</p>
      <p>Valor: R$ {{ curso.valor.replace('.', ',') }}</p>
      <p>Carga Horária: {{ curso.cargahoraria }} horas</p>
      <h2>Lista de alunos matriculados</h2>
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Código do Matrícula</th>
            <th>Nome</th>
            <th>Telefone</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="aluno in alunos" :key="aluno.codigomatricula">
            <td>{{ aluno.codigomatricula }}</td>
            <td>{{ aluno.nome }}</td>
            <td>{{ aluno.telefone }}</td>
            <td>                
                <button  class="btn btn-danger" @click="deleteMatricula(aluno.codigomatricula)">Excluir</button>
            </td>            
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        alunos: {},
        curso: {}
      };
    },
    created() {
      const codigocurso = this.$route.params.codigocurso;
      axios.get(`http://localhost:3000/alunosPorCurso/${codigocurso}`).then(response => {
        this.alunos = response.data;
      });
      axios.get(`http://localhost:3000/cursos/${codigocurso}`).then(response2 => {
        this.curso = response2.data
        this.curso = this.curso[0]
      });
    },
    methods: {
      formatarData(data) {
        const dataObj = new Date(data);
        return `${dataObj.getDate().toString().padStart(2, '0')}/${(dataObj.getMonth()+1).toString().padStart(2, '0')}/${dataObj.getFullYear()} ${dataObj.getHours()}:${dataObj.getMinutes()}`;
  
      },
      deleteMatricula(codigomatricula) { 
        axios.delete('http://localhost:3000/matriculas/'+codigomatricula).then(() => {
            // Atualiza a lista de alunos
            this.alunos = this.alunos.filter(aluno => aluno.codigomatricula !== codigomatricula);
        });                    
      },
    }
  };
  </script>