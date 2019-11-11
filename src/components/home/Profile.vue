<template>
    <div class="articles-by-User">
        <PageTitle 
             sub="Usuario" />

            <div style="padding-left:80px; margin-bottom:20px;" class="row">
                <div class="col-md-4 row">

                    <b-img class="col-md-6" left thumbnail fluid src="https://picsum.photos/125/125/?image=58" alt="Image 1"></b-img> <br>
                        <div class="col-md-12">

                         <span class="dados">{{ User.name }} </span> <br>
                         <span class="dados">{{ User.nivel }}</span>

                        </div>
                         
                </div>


                <div class="col-md-6">
                       <h5>Publique agora {{post}}!</h5> 
                       <input v-model="post" type="text">
                       <button variant="primary" @click="postar">Postar</button>
                       <h5>as{{post}}</h5>
                         <!-- <b-button variant='outline-success'>Adicionar</b-button> <br>
                         <b-button variant='outline-primary'>Sobre..</b-button> <br> -->
                         
                </div>
            </div>
            <div>
                <h5>Ultimas Postagens</h5>
            </div>
            <div>

                <hr>
                <img src="" alt="">
                <h5>as</h5> 
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


export default {
    name: 'Profile',
    components: { PageTitle },
    data: function() {
        return {
            User: {},
            articles: [],
            page: 1,
            loadMore: true,
            post:''
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
