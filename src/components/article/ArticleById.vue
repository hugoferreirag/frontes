<template>
    <div class="article-by-id">
        <PageTitle 
             sub="Editar Perfil" />
        <div class="row">


            <div class="col-md-6">

                <b-img style="margin-left:250px;" thumbnail fluid src="@/assets/Sem título.png" alt="Image 1"></b-img>

            </div>
            <div class="col-md-6">

                <h6>Nome:</h6>
                <input v-model="userers.name"  type="text">
                <h6>Email:</h6>
                <input v-model="userers.email"  type="email">
                <h6>Nascimento:</h6>
                <input v-model="userers.nascimento"  type="date">
                <h6>Data de Inicio no Ramo:</h6>
                <input v-model="userers.data_trabalho"  type="date">
                <h6>Salário:</h6>
                <input v-model="userers.salario"  type="number">
                <h6>Empresa:</h6>
                <input v-model="userers.empresa" >
                <h6>Senha:</h6>
                <input v-model="userers.password"  type="password">
                <h6>Confirme a senha:</h6>
                <input v-model="userers.confirmPassword"  type="password"> <br><hr>
                <button @click="editar">Confirmar Edição</button> <button @click="userers={}">Resetar</button>
                <b-alert v-model="showDismissibleAlert" variant="sucess" dismissible>
    Sucesso!
    </b-alert>
            </div>
        </div>

       
    </div>
</template>
<script>
import { baseApiUrl, showError, userKey } from '@/global'
import 'highlightjs/styles/dracula.css'
import hljs from 'highlightjs/highlight.pack.js'
import axios from 'axios'
import PageTitle from '../template/PageTitle'

export default {
    name: 'ArticleById',
    components: { PageTitle },
    props:['user'], 
    data: function() {
        return {
           showDismissibleAlert: false,
            userers: {
            nivel:'senior',
            name: '',
            email:'',
            nascimento:'',
            data_trabalho:'',
            empresa:'',
            password:'',
            confirmPassword:'',
            cientista:true
            }
        }
    },
       computed:{
        user(){
            return this.$store.state.user
        }
    },
    methods:{
     showAlert() {
        this.dismissCountDown = this.dismissSecs
      },
        editar(){
                axios.put(`${baseApiUrl}/users/${this.user.id}`, this.userers)
                .then(() => {
                    this.showDismissibleAlert=true;
                    this.$router.push({ name: 'home' });
                    this.$toasted.global.defaultSuccess()
                    this.userers = {}
                    
                  

               
                })
                .catch(showError)
        }
    },
    mounted() {
        const url = `${baseApiUrl}/articles/${this.$route.params.id}`
        axios.get(url).then(res => this.article = res.data)
        this.userers ={
            
            name: this.user.name,
            email:this.user.email,
            nascimento:this.user.nascimento,
            data_trabalho:this.user.trabalho_data,
            salario:this.user.salario,
            empresa:this.user.empresa,
            password:'',
            confirmPassword:''}
    },
    updated() {
        document.querySelectorAll('.article-content pre.ql-syntax').forEach(e => {
            hljs.highlightBlock(e)
        })
    }
}
</script>

<style>
    input{
        border-radius:5px;
        font-family: 'Courier New', Courier, monospace,;
        width:225px
    }
    h6{
        font-family: 'Courier New', Courier, monospace
    }
    .article-content {
        background-color: #FFF;
        border-radius: 8px;
        padding: 25px;
    }

    .article-content pre {
        padding: 20px;
        border-radius: 8px;
        font-size: 1.2rem;
        background-color: #1e1e1e;
        color: #FFF;
    }

    .article-content img {
        max-width: 100%;
    }

    .article-content :last-child {
        margin-bottom: 0px;
    }
</style>
