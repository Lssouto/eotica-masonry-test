<template>
  <div id="app" class="bg-light-gray">
    <c-header @put-search="putSearch"/>
    <list :items="list.slice(0,itemsPerPage*page)"/>
    <div class="text-center" v-if="!list.length"> 
      <h2>Nenhum item encontrado!!</h2>
    </div>
    <div class="text-center" v-if="page*itemsPerPage < list.length">
      <i class="fa fa-spinner fa-pulse fa-2x fa-fw"></i>
      <span class="sr-only">Page Loading...</span>
      Page Loading . . .
    </div>
  </div>
</template>

<script>
import cHeader from './components/Header'
import List from './components/list'
export default {
  name: 'app',
  components: {
    cHeader,
    List
  },
  data(){
    return{
      list: [],
      backup: [],
      itemsPerPage: 8,
      page: 0,
      searchParams: ''
    }
  },
  beforeMount () {
    window.addEventListener('scroll', this.handleScroll);
  },
  beforeDestroy () {
    window.removeEventListener('scroll', this.handleScroll);
  },
  mounted (){
    this.searchParams =  window.location.search.split('=')[1];
    this.feed()
  },
  methods: {
    feed(){
      this.$http.get('https://next.json-generator.com/api/json/get/4k8rec4qE').then(response => {
        this.backup = response.body;
        this.formatItems();
        this.filterItems();
      }, response =>{
        console.log('error' + response)
      })
    },
    formatItems (){
      //Json string then json parse is to remove the Vue Object Biding, keep backup as a backup
      this.page = 1;
      if(this.backup.length < this.itemsPerPage){
        this.list = JSON.parse(JSON.stringify(this.backup)).slice(0,this.backup.length);
      }
      else{
        this.list = JSON.parse(JSON.stringify(this.backup)).slice(0, this.itemsPerPage);
      }
    },
    putSearch(value){
      //filtro manual
      this.searchParams = value;
      this.filterItems();
      //Normalmente é requisição pro back
    },
    filterItems(){
      const filter = this.searchParams;
      if(!filter){
        this.list = JSON.parse(JSON.stringify(this.backup))
      }
      else{
        
        let newItems = this.backup.filter(item=>{
          return (item.locale).indexOf(filter) !== -1 || (item.title).indexOf(filter) !== -1 || (item.description).indexOf(filter) !== -1;
        })
        this.list = newItems;
      }
    },
    handleScroll (){
      if ((window.innerHeight + window.scrollY) >= document.body.offsetHeight){
        if(this.page*this.itemsPerPage < this.backup.length)
          this.loadItems();
      }
    },
    loadItems (){
      this.page +=1;
      this.filterItems();
    }
  } 
}
</script>

<style lang="scss">
#app{
  padding-bottom: 25px;
}
</style>
