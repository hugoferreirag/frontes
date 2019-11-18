<template>
    <div class="auth-content">
        <div class="auth-modal">
            <img v-if="showSignup" src="@/assets/logo.png" width="100" alt="Logo" />
             <img v-if="!showSignup" class="imagem" src="@/assets/Sem título.png" width="200" alt="Logo" />
            <hr>
            <div class="h4 text-center mb-4">{{ showSignup ? 'Sign Up' : 'Log In' }}</div>
             <label  v-if="!showSignup" for="text-password">Email:</label>
            <input icon="envelope"  v-if="!showSignup" v-model="user.email" name="email" type="text" placeholder="E-mail">
              <label   v-if="!showSignup" for="text-password">Senha:</label>
             <input     v-if="!showSignup" v-model="user.password" name="password" type="password" placeholder="Senha">
            
            
          
        
            
            <div v-else></div>
           
            <div v-if="showSignup && user.cientista===true">
            <label for="cientista">É um cientista da Computação ?</label> 
             <label v-if="showSignup" for="cientista">É um cientista da Computação ?</label> 
             <input v-model="user.cientista" v-if="showSignup" name="cientista" type="checkbox" placeholder=""> 
            <label  for="text-password">Nome:</label>
            <input v-model="user.name" type="text" placeholder="Nome">
      
            <label  for="text-password">Email:</label>
            <input  icon="envelope" v-model="user.email" name="email" type="text" placeholder="E-mail">
             <label for="text-password">Senha:</label>
             <input    v-model="user.password" name="password" type="password" placeholder="Senha">
               <label  for="text-password">Confirmar Senha:</label>
               <input  v-model="user.confirmPassword"
                type="password" placeholder="Confirme a Senha">     

              <label   for="">Data de nascimento</label>
              <input   v-model="user.nascimento" name="data" type="date" placeholder="Data de Nascimento">
              <label for="">Quando começou no ramo da Ciência da computação??</label>
              <input  v-model="user.trabalho_data" name="data" type="date" placeholder="">
              <label   for="">Empresa Atual</label>
              <input  v-model="user.empresa" name="empresa" type="text" placeholder="Nome da atual empresa">
               <label   for="">Nivel de Instrução</label>
              <input  v-model="user.nivel" name="nivel" type="text" placeholder="Nivel de instrução">
               <label   for="">Salário Atual</label>
              <input  v-model="user.salario" name="salario" type="number" placeholder="Salário atual">

            </div>
            <button v-if="showSignup && user.cientista===true"   @click="signup">Registrar</button>
            <button v-if="!showSignup" @click="signin">Entrar</button>

            <a style="text-align:center" href @click.prevent="showSignup = !showSignup">
                <span v-if="showSignup">Já tem cadastro? Acesse o Login!</span>
                <span  v-else>Não tem cadastro? Registre-se gratuitamente!</span>
               
            </a>
            <a style="text-align:center" href @click.prevent="showPremium = !showPremium">
                <span v-if="showSignup"></span>
                <span  v-else>Ou assine a versão premium!</span>
               
            </a>
        </div>
    </div>
</template>

<script>
import { baseApiUrl, showError, userKey } from '@/global'
import axios from 'axios'
export default {
    name: 'Auth',
    data: function() {
        return {
            showSignup: false,
            user: {},
            showPremium:false
        }
    },
    methods: {
        signin() {
            axios.post(`${baseApiUrl}/signin`, this.user)
                .then(res => {
                    this.$store.commit('setUser', res.data)
                    localStorage.setItem(userKey, JSON.stringify(res.data))
                    this.$router.push({ path: '/' })
                })
                .catch(
                
                )
        },
        signup() {
            axios.post(`${baseApiUrl}/signup`, this.user)
                .then(() => {
                    this.$toasted.global.defaultSuccess()
                    this.user = {}
                    this.showSignup = false
                })
                .catch(showError)
        }
    }
}
</script>

<style scoped>
.auth-content {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
 
}
.auth-modal {
  background-color: white;
  width: 350px;
  padding: 35px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 5px;
}
.image {
  border-radius: 200px !important;
}
.auth-title {
  font-size: 1.2rem;
  font-weight: 100;
  margin-top: 10px;
  margin-bottom: 15px;
}
.auth-modal input {
  border: 1px solid #bbb;
  width: 100%;
  margin-bottom: 15px;
  padding: 3px 8px;
  outline: none;
}
.auth-modal button {
  align-self: flex-end;
  background-color: #2460ae;
  color: #fff;
  padding: 5px 15px;
}
.auth-modal a {
  margin-top: 35px;
}
.auth-modal hr {
  border: 0;
  width: 100%;
  height: 1px;
  background-image: linear-gradient(
    to right,
    rgba(120, 120, 120, 0),
    rgba(120, 120, 120, 0.75),
    rgba(120, 120, 120, 0)
  );
}
</style>
