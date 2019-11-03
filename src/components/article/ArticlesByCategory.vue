<template>
    <div class="articles-by-User">
        <PageTitle 
             sub="Usuario" />

            <div class="row">
                <div class="col-md-12">

                    <b-img center thumbnail fluid src="https://picsum.photos/125/125/?image=58" alt="Image 1"></b-img>

                </div>

                <div class="col-md-12">
                         <span>Nome: {{User.name}} </span> 
                         <span>Data de nascimento : {{ new Date(User.nascimento).toLocaleDateString() }} </span>
                         <span>Inicio no ramo: {{new Date(User.trabalho_data).toLocaleDateString()}}</span> 
                         <span>Email : {{User.email}} </span>
                         <span>É um cientista? : {{User.cientista == true? 'sim' : 'Não'}}</span> 
                         <span>Salário : {{User.salario}}</span> 
                </div>
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
</style>
