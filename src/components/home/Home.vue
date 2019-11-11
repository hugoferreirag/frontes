<template>
    <div class="home">
      
    
        <PageTitle icon="fa fa-home" main="Dashboard"
            sub="Rede Linked" />
          
            
        <div class="stats">
            <Stat title="Vagas" :value="stat.categories"
                icon="fa fa-folder" color="#d54d50" />
            <Stat title="Noticias" :value="stat.articles"
                icon="fa fa-file" color="#3bc480" />
            <Stat title="Cientistas" :value="stat.users"
                icon="fa fa-user" color="#3282cd" />
        </div>
    </div>
</template>

<script>
import PageTitle from '../template/PageTitle'
import Stat from './Stat'
import axios from 'axios'
import { baseApiUrl } from '@/global'

export default {
    name: 'Home',
    components: { PageTitle, Stat },
    data: function() {
        return {
            stat: {},
         
        }
    },
    computed:{
        user(){
            return this.$store.state.user
        }
    },
    methods: {
        getStats() {
            axios.get(`${baseApiUrl}/stats`).then(res => this.stat = res.data)
        },
        editar(){
             this.$router.push({ path:`/editar/${this.user.id}` });
        }
    },
    mounted() {
        this.getStats()
    }
}
</script>

<style>
    .stats {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
    }
</style>
