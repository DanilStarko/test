<template>
  <div id="app">
    <header class="header">
       <div class="header__body">
          <ul class="header__nav">
            <li 
               class="header__item"
               v-for="(nav, index) of navs"
               :key="index"
            >
               <a class="header__link"
                  href="#"
                  :class="{'active': index === isActive}"
                  @click="currentNav(nav, index)"
               >
               {{ nav }}
               </a>
            </li>
         </ul>
       </div>
    </header>
    <component :is="currentView"></component>

  </div>
</template>

<script>

import createUser from './components/createUser'
import game from './components/game'

export default {
   components: {
      createUser, game
   },
   name: 'App',
   data() {
      return {
         currentView: 'createUser',
         navs: ['форма', 'игра'],
         isActive: 0
      }
      
   },
   methods: {
      currentNav(nav, index) {
         this.isActive = index
         switch (nav) {
            case this.navs[0]:
               this.currentView = 'createUser'
               break;
            case this.navs[1]:
               this.currentView = 'game'
               break;
         }
      }
   }

}
</script>

<style lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&display=swap');

*{padding:0;margin:0;border:0;}
*,*:before,*:after{-moz-box-sizing:border-box;-webkit-box-sizing:border-box;box-sizing: border-box;}
a:focus,a:active{outline: none; text-decoration: none;}
input,button,textarea{font-family:inherit;}
a{text-decoration: none;}
a:hover{text-decoration: none;}
ul li{list-style: none;}


body {
   padding: 0;
   margin: 0;
    -webkit-box-sizing: border-box; -moz-box-sizing: border-box; box-sizing: border-box;
}

#app {
   font-family: 'Montserrat', sans-serif;
   color: #000;
   font-size: 16px;
   line-height: 18px;
}

.header {
   &__body {
      max-width: 300px;
      margin: 0 auto;
      padding: 30px 15px;
   }
   &__nav{
      display: flex;
      justify-content: space-between;
      font-size: 18px;
      line-height: 22px;
      font-weight: 600;
      padding: 0;
   }
   &__item {
      list-style: none;

   }
   &__link {
      color: #000;
      border: 2px solid #000;
      background-color: blue;
      transition: all 0.3s;
      padding: 10px;
      min-width: 100px;
      color: #fff;
      display: block;
      text-align: center;
      &:hover{
      }
      &.active {
         background-color: green;
      }
   }
}

</style>
