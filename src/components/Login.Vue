<template>
    <div>
      <form @submit.prevent="login">
        <div>
          <label for="email">Email:</label>
          <input type="email" id="email" v-model="email" required class="form-control">
        </div>
        <div>
          <label for="senha">Senha:</label>
          <input type="password" id="senha" v-model="senha" required class="form-control">
        </div>
        <br>
        <div>
          <button type="submit" class="btn btn-primary">Entrar</button>
        </div>
      </form>
      <div v-if="erro">
        {{ erro }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        email: '',
        senha: '',
        erro: ''
      };
    },
    methods: {
      async login() {
        try {
          const response = await fetch('http://localhost:3000/login', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              email: this.email,
              senha: this.senha
            })
          });
          if (response.ok) {
            const data = await response.json();
            localStorage.setItem('token', data.token);
            this.$router.push('/');
            
          } else {
            throw new Error('Credenciais inválidas');
          }
        } catch (error) {
          this.erro = error.message;
        }
      }
    }
  };
  </script>