<template>
  <div id="app">
    <header>
      <div class="container">
        <div class="flex-align">
          <div class="container-logo">
            <img src="../assets/images/logo.png" alt="">
          </div>
          <div class="input-container">
            <input type="text" class="form-control" v-model="search" id="search">
            <label for="search">
              <span class="fa fa-search inside"></span>
            </label> 
          </div>
          <div class="container-user desktop-only">
            <div class="circle">
                <img src="../assets/images/user-icon.png" alt="">
            </div>
            <span>Welcome Junior</span>
          </div>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
export default {
    name: 'c-header',
    data(){
        return{
            search: ''
        }
    },
    watch: {
        search: function(value){
            if(!!value)
                window.history.pushState({},'', "?search="+value);
                else
                    window.history.pushState({},'', "?"); 
            this.$emit('put-search', value)
        }
    },
    mounted (){
      this.search = window.location.search.split('=')[1]
    }
}
</script>

<style lang="scss">

</style>
