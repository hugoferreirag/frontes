<template>
    <div class="articles-by-User">
        <PageTitle 
             sub="Usuario" />

            <div style="padding-left:80px" class="row">
                <div class="col-md-4 row">

                         <b-img class="col-md-12" left thumbnail fluid src="https://picsum.photos/80/80/?image=58" alt="Image 1"></b-img>

                </div>

                <div class="col-md-6">
                         <b-button variant='outline-success'>Adicionar</b-button> 
                         <br> <hr>
                         <span class="dados">Nome: {{ User.name }} </span> <br>
                         <span class="dados">Senioridade: {{ User.nivel }}</span><br>
                         <span class="dados">Empresa: {{ User.empresa }}</span><br>
                         <span class="dados">Inicio da Carreira: {{ new Date(User.trabalho_data).toLocaleDateString() }}</span> <br>
                         <span class="dados">Email :{{ User.email }}</span><br>
                </div>

            </div>
            <hr>
            <hr>
             <div class="col-md-12">
                <h5>Ultimas Postagens</h5>
            </div>
            <div>

                <hr>
                <img src="https://picsum.photos/125/125/?image=58" alt="">
                <h5>Sou um progamador e tanto ! Sei node é Vue</h5> 
                <hr>

                <hr>
                <img src="https://picsum.photos/125/125/?image=58" alt="">
                <h5>Desenvolva e evolua !!!</h5> 
                <hr>

                <hr>
                <img src="https://picsum.photos/125/125/?image=58" alt="">
                <h5>Viva a tecnologia !!</h5> 
                <hr>
            </div>
        <div class="load-more">
            <button v-if="loadMore"
                class="btn btn-lg btn-outline-primary"
                @click="getArticles">Carregar Mais Artigos</button>
        </div>
    </div>
</template>

<script>
import { baseApiUrl } from '@/global'
import axios from 'axios'
import PageTitle from '../template/PageTitle'
import ArticleItem from './ArticleItem'

export default {
    name: 'ArticlesByCategory',
    components: { PageTitle, ArticleItem },
    data: function() {
        return {
            User: {},
            articles: [],
            page: 1,
            loadMore: true
        }
    },
    methods: {
        getUser() {
            const url = `${baseApiUrl}/users/${this.User.id}`
            axios(url).then(res => this.User = res.data)
        },
        getArticles() {
            const url = `${baseApiUrl}/categories/${this.User.id}/articles?page=${this.page}`
            axios(url).then(res => {
                this.articles = this.articles.concat(res.data)
                this.page++

                if(res.data.length === 0) this.loadMore = false
            })
        }
    },
    watch: {
        $route(to) {
            this.User.id = to.params.id
            this.articles = []
            this.page = 1
            this.loadMore = true

            this.getUser()
            this.getArticles()
        }
    },
    mounted() {
        this.User.id = this.$route.params.id
        this.getUser()
        this.getArticles()
    }
}
</script>

<style>
    .articles-by-User ul {
        list-style-type: none;
        padding: 0px;
    }

    .articles-by-User .load-more {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 25px;
    }
    .dados{
        font-family: 'Courier New', Courier, monospace;
        font-size:15px
    }
</style>
