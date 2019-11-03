<template>
    <div class="article-by-id">
        <div class="row">


            <div class="col-md-6">

                <b-img thumbnail fluid src="~@/assets/img/nene.jpg" alt="Image 1"></b-img>

            </div>
            <div class="col-md-6">

                <h5>Nome:</h5>
                <input v-model="userers.name"  type="text">
                <h5>Email:</h5>
                <input v-model="userers.email"  type="text">
                <h5>Nascimento:</h5>
                <input v-model="userers.nascimento"  type="date">
                <h5>Data de Inicio no Ramo:</h5>
                <input v-model="userers.data_trabalho"  type="date">
                <h5>Salário:</h5>
                <input v-model="userers.salario"  type="number">
                <h5>Empresa:</h5>
                <input v-model="userers.empresa"  type="text">
                <h5>Senha:</h5>
                <input v-model="userers.password"  type="password">
                <h5>Confirme a senha:</h5>
                <input v-model="userers.confirmPassword"  type="password">
                <button @click="editar">Confirmar Edição</button> <button @click="userers={}">Resetar</button>
                
            </div>
        </div>

       
    </div>
</template>

<script>
import 'highlightjs/styles/dracula.css'
import hljs from 'highlightjs/highlight.pack.js'
import { baseApiUrl } from '@/global'
import axios from 'axios'
import PageTitle from '../template/PageTitle'

export default {
    name: 'ArticleById',
    components: { PageTitle },
    props:['user'],
    data: function() {
        return {
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
        editar(){
                axios.put(`${baseApiUrl}/users/${this.user.id}`, this.userers)
                .then(() => {
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
            nascimento:'',
            data_trabalho:'',
            salario:0,
            empresa:'',
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
        font-family: 'Courier New', Courier, monospace,
    }
    h5{
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
