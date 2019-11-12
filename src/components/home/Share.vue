<template>
    <div class="articles-by-User">
        <PageTitle 
             sub="Cientistas" />
             <hr>
       <input style="margin-left:50px" v-model="sharear" placeholder="Pesquise Cientistas.. " type="text">
            <button variant="primary" @click="getShares" >Pesquisar</button>
      
            <div style="margin-left:120px;cursor:pointer" v-for="value in usuarios" :key="value.id">

                <hr>
                <div @click="pushUser(value.id)">
                    <img src="https://picsum.photos/80/80/?image=58" alt="">
                    <h5>{{value.name}}</h5> 
                </div>
                <hr>
            </div>
        
    </div>
</template>

<script>
import { baseApiUrl } from '@/global'
import axios from 'axios'
import PageTitle from '../template/PageTitle'


export default {
    name: 'Share',
    components: { PageTitle },
    props:['users'],
    data: function() {
        return {
            usuarios:{},
            sharear:''
        }
    },
    methods: {
        // getUser() {
        //     const url = `${baseApiUrl}/users/${this.User.id}`
        //     axios(url).then(res => this.User = res.data)
        // },
        pushUser(item){
          
               this.$router.push({
                name: 'articlesByCategory',
                params:{id:item}
            })
            
        },
        getShares() {
            console.log(this.sharear)
            const url = `${baseApiUrl}/usersshare/${this.sharear}`
            axios(url).then(res =>{
                console.log(res)
                this.usuarios = res.data
            })
        }
    },
    
    mounted() {
        
        console.log(this.users)
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
