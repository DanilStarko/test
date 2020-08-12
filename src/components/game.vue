<template>
   <div class="game">
      <div class="game__row">
         <div class="game__left">
            <div class="game__field field">
               <ul class="field__list" id="field">
                  <li 
                     v-for="(item, index) of Idlist"
                     :key="index"
                     class="field__item" 
                     :id="item"
                     @click="clickUser(item)"
                  >
                  </li>
               </ul>
            </div>
         </div>
         <div class="game__right">
            <div class="game__panel panel">
               <div class="panel__rounds">Round: <span>{{ rounds }}</span></div>
               <div 
                  class="panel__btn"
                  @click="startGame"
                  id="btn"
               >Start</div>
               <div class="panel__state">status: <span>{{ status }}</span></div>
               <div class="panel__mode">Levels</div>
               <div class="panel__levels">
                  <div 
                     class="panel__level"
                     v-for="(level, index) of levels"
                     :key="index"
                  >
                     <input 
                        @click="changeState" 
                        type="radio" 
                        id="level" 
                        v-model="picked" 
                        :value="level"
                     >
                     <label for="level">{{ level }}</label>
                  </div>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>

<script>
   export default {
      name: 'game',
      data() {
         return {
            levels: ['Normal', 'Middle', 'Hight'],
            start: false,
            rounds: 0,
            status: '',
            picked: '',
            Idlist: [0, 1, 2, 3],
            randomArray: [],
            i: 0,
            timeOut: null
         }
      },
      methods: {
         startGame() {
            this.changeState()
            if (this.picked) {
               if (this.start == false) {
                  this.gameInit()
               }
            }else {
               this.status = 'Please choose level the game'
            }
         },
         gameInit() {
            this.start = true
            this.i = 0
            this.rounds = ++this.rounds
            this.randomArray = []
            for (let i = 0; i < this.rounds; i++) {
                let currentItem = Math.floor(Math.random() * this.Idlist.length);
                this.randomArray.push(currentItem)
            }
            this.viewRandomArray()
         },
         viewRandomArray() {
            if (this.picked == 'normal') this.timeOut = 1500
            else if (this.picked == 'middle') this.timeOut = 1000
            else this.timeOut = 400
            let interval = setInterval(() => {
               if (this.i >= this.randomArray.length) {
                  clearInterval(interval)
               } else {
                  this.opacityUp()
               }
            }, this.timeOut)
            
         },
         opacityUp() {
            let elem = document.getElementById(this.randomArray[this.i])
            elem.style.opacity = 1
            setTimeout(() => elem.style.opacity = 0.6, 500)
            this.i++
         },
         clickUser(item) {
            if (this.start) {
               //this.isActive(item)
               if (item == this.randomArray[0]) {
                  this.randomArray.shift()
                  
                  if (!this.randomArray.length){
                     this.endRound()
                  }
                  
               } else {
                  this.status = 'Вы проиграли на раунде ' + this.rounds
                  this.start = false
                  this.rounds = 0
               }
            } else {

               return
            }
            
         },
         endRound () {
            this.gameInit()
         },
         changeState () {
            this.status = ''
         }
      }
   }
</script>

<style lang="scss">
.game {
   width: 100%;
   max-width: 800px;
   margin: 0px auto;
   padding: 100px 0px 0px 0px;
		// .game__row

		&__row {
         display: flex;
         align-items: center;
		}

}
.field {
   width: 400px;
   height: 400px;
		// .field__list

		&__list {
         width: 100%;
         height: 100%;
         display: flex;
         flex-wrap: wrap;
         :nth-child(1) {
            background: red;
         }
         :nth-child(2) {
            background: green;
         }
         :nth-child(3) {
            background: blue;
         }
         :nth-child(4) {
            background: yellow;
         }
		}

		// .field__item

		&__item {
         flex: 0 0 50%;
         height: 50%;
         opacity: 0.6;
         cursor: pointer;
         &.active{
            opacity: 1 !important;
         }
		}
}
.panel {
   padding: 0px 0px 0px 50px;
		// .panel__rounds

		&__rounds {
         font-size: 22px;
         line-height: 26px;
         font-weight: 700;
         margin: 0px 0px 20px 0px;
		}

		// .panel__btn

		&__btn {
         width: 100px;
         height: 50px;
         border-radius: 30px;
         background-color: #ccc;
         font-size: 22px;
         line-height: 26px;
         margin: 0px 0px 20px 0px;
         display: flex;
         justify-content: center;
         align-items: center;
         cursor: pointer;
		}

		// .panel__state

		&__state {
         font-size: 16px;
         line-height: 18px;
         margin: 0px 0px 20px 0px;
         color: green;
         span {
            color: #000
         }
		}
      &__mode {
         font-size: 22px;
         line-height: 26px;
         font-weight: 500;
         margin: 0px 0px 15px 0px;
      }
		// .panel__levels

		&__levels {
         padding: 0px 0px 0px 20px;
         font-size: 16px;
         line-height: 18px;
         margin: 0px 0px 20px 0px;
		}
      &__level {
         margin: 0px 0px 10px 0px;
         input {
            margin: 0px 10px 0px 0px;
         }
		}
}

</style>