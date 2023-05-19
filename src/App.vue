<template>
  
  <header class="header">
    <img src="@/assets/acme.jpg" alt="ACME" class="header-logo">      
  </header>
  <div v-if="usuarioLogado">
  <Slide ref="slide">
    <router-link to="/" @click="closeMenu()">
      <span>Início</span>
    </router-link>        
    <router-link to="/alunos" @click="closeMenu()">
      <span>Alunos</span>
    </router-link> 
    <router-link to="/cursos" @click="closeMenu()">
      <span>Cursos</span>
    </router-link> 
    <router-link to="/matriculas" @click="closeMenu()">
      <span>Matrículas</span>
    </router-link> 
    <router-link to="/login" @click="sair()">
      <span>Sair</span>
    </router-link> 
  </Slide>      
</div>
<br>
<router-view />  
<br>
<footer class="footer">
  <p>SENAC. &copy; 2023 Todos os direitos reservados.</p>
</footer>       
</template>

<script>
import { Slide } from 'vue3-burger-menu'

export default {
components: {
  Slide
},
data() {
  return {
    usuarioLogado: true
  }
},
methods: {
  openMenu() {
    //this.$refs.slide.toggle() // abre o menu
  },
  closeMenu() {       
    //this.$refs.slide.toggle() // fecha o menu
  },
  sair() {       
    localStorage.removeItem('token')
    this.$router.push('/login')
    this.usuarioLogado = false
  },
  verificarAutenticacao() {
    const token = localStorage.getItem('token') // recupera o token do local storage

    fetch('http://localhost:3000/autenticado', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ token: token }) // envia o token no corpo da requisição
    })
    .then(response => {
      if (response.ok) {
        console.log('resposta ok autenticação');
        this.usuarioLogado = true // atualiza a variável para verdadeiro se a resposta for ok
      } else {
        console.log('resposta Não autenticação - remover token');
        this.usuarioLogado = false
        localStorage.removeItem('token') // remove o token do local storage se a resposta não for ok
        this.$router.push('/login')
      }
    })
    .catch(error => {
      console.error(error)
    })
  },
},
mounted() {
  this.verificarAutenticacao() // chama a função de verificação da autenticação no momento que o componente é montado
},
created() {
  this.$router.beforeEach((to, from, next) => {
    this.verificarAutenticacao()
    next()
  })
}
}
</script>



<style>
.header {
display: flex;
justify-content: center;
align-items: center;
height: 50px;
background-color: #000000;
margin-top: 80px;
}

.header-logo {
width: 100px;
height: auto;
object-fit: contain;
}

.footer {
display: flex;
justify-content: center;
align-items: center;
height: 50px;
background-color: #f0f0f0;
}

.bm-burger-button {
  position: flex;
  width: 36px;
  height: 30px;
  left: 36px;
  top: 36px;
  cursor: pointer;
}
.bm-burger-bars {
  background-color: #000000;
}
.line-style {
  position: absolute;
  height: 20%;
  left: 0;
  right: 0;
}
.cross-style {
  position: absolute;
  top: 12px;
  right: 2px;
  cursor: pointer;
}
.bm-cross {
  background: #bdc3c7;
}
.bm-cross-button {
  height: 24px;
  width: 24px;
}
.bm-menu {
  height: 100%; /* 100% Full-height */
  width: 0; /* 0 width - change this with JavaScript */
  position: fixed; /* Stay in place */
  z-index: 1000; /* Stay on top */
  top: 0;
  left: 0;
  background-color: rgb(0, 0, 0); /* Black*/
  overflow-x: hidden; /* Disable horizontal scroll */
  padding-top: 60px; /* Place content 60px from the top */
  transition: 0.5s;
}

.bm-overlay {
  background: rgba(254, 254, 254, 0.3);
}
.bm-item-list {
  color: #b8b7ad;
  margin-left: 10%;
  font-size: 20px;
}
.bm-item-list > * {
  display: flex;
  text-decoration: none;
  padding: 0.7em;
}
.bm-item-list > * > span {
  margin-left: 10px;
  font-weight: 700;
  color: white;
}
</style>