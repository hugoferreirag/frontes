<template>
    <aside class="menu" v-show="isMenuVisible">
    <br>
    <br>
    <div>
  <b-nav vertical class="w-25">
    <b-nav-item active href="/shares"><img class="teste" src="@/assets/Sem título.png"></b-nav-item>
    <b-nav-item  color ="blue" active href="/shares">Pesquisar Cientistas</b-nav-item>
    <b-nav-item color ="blue" active href="/shares">Acessar Feed</b-nav-item>
   
  </b-nav>
</div>
      

        
        
    </aside>
</template>

<script>
import { mapState } from 'vuex'
import Tree from 'liquor-tree'
import { baseApiUrl } from '@/global'
import axios from 'axios'

export default {
    name: 'Menu',
    components: { Tree },
    computed: mapState(['isMenuVisible']),
    data: function() {
        return {
            treeFilter: '',
            treeData: this.data(),
            treeOptions: {
                propertyNames: { 'text': 'name' },
                filter: { emptyText: 'Usuário não encontrado' }
            }
        }
    },
 
    methods: {
     perfil(){
             this.$router.push({ path:`/profile/${this.user.id}` });
        },
        data() {
            const url = `${baseApiUrl}/users`
            return axios.get(url).then(res => res.data)
        },
        onNodeSelect(node) {
            this.$router.push({
                name: 'articlesByCategory',
                params: { id: node.id }
            })

            if(this.$mq === 'xs' || this.$mq === 'sm') {
                this.$store.commit('toggleMenu', false)
            }
        },
              share(){
            
               this.$router.push({
                name: 'shares'
            })
            
        }
    },
    mounted() {
        this.$refs.tree.$on('node:selected', this.onNodeSelect)
    }
}
</script>

<style>
    .menu {
        grid-area: menu;
        background: linear-gradient(to right, #232526, #414345);
        width:200px;
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
    }

    .menu a,
    .menu a:hover {
        color: #fff;
        text-decoration: none;
    }

    .menu .tree-node.selected > .tree-content,
    .menu .tree-node .tree-content:hover {
        background-color: rgba(255, 255, 255, 0.2);
    }

    .tree-arrow.has-child {
        filter: brightness(2);
    }
    .teste{
    width:150px !important;
    border-radius:100px;
    }

    .menu .menu-filter {
        display: flex;
        justify-content: center;
        align-items: center;

        margin: 20px;
        padding-bottom: 8px;
        border-bottom: 1px solid #AAA;
    }

    .menu .menu-filter i {
        color: #AAA;
        margin-right: 10px;
    }

    .menu input {
        color: #CCC;
        font-size: 1.3rem;
        border: 0;
        outline: 0;
        width: 100%;
        background: transparent;
    }

    .tree-filter-empty {
        color: #CCC;
        font-size: 1.3rem;
        margin-left: 20px;
    }
</style>
