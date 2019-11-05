<template>
    <div class="auth-content">
        <div class="auth-modal">
            <img v-if="showSignup" src="@/assets/logo.png" width="200" alt="Logo" />
             <img v-if="!showSignup" src="@/assets/Sem título.png" width="200" alt="Logo" />
            <hr>
            <div class="h4 text-center mb-4">{{ showSignup ? 'Cadastro' : 'Login' }}</div>
             <label v-if="showSignup" for="cientista">É um cientista da Computação ?</label> 
             <input v-model="user.cientista" v-if="showSignup" name="cientista" type="checkbox" placeholder=""> 
            <input v-if="showSignup && user.cientista===true"  v-model="user.name" type="text" placeholder="Nome">
            <input icon="envelope" v-model="user.email" name="email" type="text" placeholder="E-mail">
             <input v-model="user.password" name="password" type="password" placeholder="Senha">
               <input v-if="showSignup && user.cientista===true"  v-model="user.confirmPassword"
                type="password" placeholder="Confirme a Senha">

            <label v-if="showSignup" for="">Quando começou no ramo da Ciência da computação?</label>
            <input v-if="showSignup" v-model="user.trabalho_data" name="data" type="date" placeholder="">
            <input v-if="showSignup" v-model="user.empresa" name="empresa" type="text" placeholder="Nome da atual empresa">
            <input v-if="showSignup" v-model="user.nivel" name="nivel" type="text" placeholder="Nivel de instrução">
            <input v-if="showSignup" v-model="user.salario" name="salario" type="number" placeholder="Salário atual">
            <button v-if="showSignup" @click="signup">Registrar</button>
             
            
            <label v-if="showSignup && user.cientista===true"  for="">Data de nascimento</label>
            <input v-if="showSignup && user.cientista===true"  v-model="user.nascimento" name="data" type="date" placeholder="Data de Nascimento">
              
           
            
            <div v-else></div>
           
          
            <label v-if="showSignup && user.cientista===true" for="">Quando começou no ramo da Ciência da computação??</label>
            <input v-if="showSignup && user.cientista===true"  v-model="user.trabalho_data" name="data" type="date" placeholder="">
            <input v-if="showSignup && user.cientista===true"  v-model="user.empresa" name="empresa" type="text" placeholder="Nome da atual empresa">
            <input v-if="showSignup && user.cientista===true"  v-model="user.nivel" name="nivel" type="text" placeholder="Nivel de instrução">
            <input v-if="showSignup && user.cientista===true"  v-model="user.salario" name="salario" type="number" placeholder="Salário atual">
            <button v-if="showSignup && user.cientista===true"   @click="signup">Registrar</button>
            <button v-if="user.cientista===false" disabled @click="signup">Registrar</button>
            <button v-else @click="signin">Entrar</button>

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

<style>
    .auth-content {
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        background: #2460ae;
    }
    .auth-modal {
        background-color:red;
        width: 350px;
        padding: 35px;
        box-shadow: 0 1px 5px rgba(0, 0, 0, 0.15);
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .auth-title {
        font-size: 1.2rem;
        font-weight: 100;
        margin-top: 10px;
        margin-bottom: 15px;
    }
    .auth-modal input {
        border: 1px solid #BBB;
        width: 100%;
        margin-bottom: 15px;
        padding: 3px 8px;
        outline: none;
    }
    .auth-modal button {
        align-self: flex-end;
        background-color: #2460ae;
        color: #FFF;
        padding: 5px 15px;
    }
    .auth-modal a {
        margin-top: 35px;
    }
    .auth-modal hr {
        border: 0;
        width: 100%;
        height: 1px;
        background-image: linear-gradient(to right,
            rgba(120, 120, 120, 0),
            rgba(120, 120, 120, 0.75),
            rgba(120, 120, 120, 0));
    }
</style>
