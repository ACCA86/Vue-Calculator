<template>
  <div id="con-calculate">

      <div class="con-result">
        <input type="submit" :value="DR" @click='SwitchDR'>
        <input type="text" class="res" placeholder="0" v-model="cal" readonly>
        <input  class="text"  value="{result}" v-model="result" readonly>
      </div>

      <div>
        <div class="show">
          <i class="fa fa-angle-double-down" aria-hidden="true"></i>
        </div>
        <div class="con-btn">
        <input
        type="submit"
        v-for="item in obj"
        :value="item"
        :key="item"
        @click="action(item)"
        :class="{'dark-vue':['π','%','√','x​²','x³','x^','COS','SIN','TAN','÷','-','÷','×','+'].includes(item),
        'rest-btn':['7','8','9','4','5','6','1','2','3','0','.'].includes(item),
        'purple-vue':['DEG','RAD'].includes(item),
        'delete-vue':['C','AC'].includes(item),
        'blue-vue':['='].includes(item)}">
        </div>
      </div>

  </div>
</template>
<script>

export default {
  data(){
    return{
      cal:'',
      result:'',
      DR:'DEG',
      obj:['π','%','√','x​²','x³','x^','COS','SIN','C','AC','TAN','÷','7','8','9','×','4','5','6','-','1','2','3','+','0','.','='],
      operator:'',
      prevoperator:'',
      pi:'3.141592653589793',
      sys:Math.PI/180,
      pyp:Math.PI
    }
  }
  ,
  methods:{
    SwitchDR(){
      this.DR = this.DR === 'DEG' ? 'RED' : 'DEG'
      if(this.DR === 'RED'){
        if(this.cal.includes('cos')){this.result = Math.cos(this.cal.slice(4,20))}
        if(this.cal.includes('sin')){this.result = Math.cos(this.cal.slice(4,20))}
        if(this.cal.includes('tan')){this.result = Math.cos(this.cal.slice(4,20))}
      }
      if(this.DR === 'DEG'){
        if(this.cal.includes('cos')){this.result = Math.cos(this.cal.slice(4)*this.sys)}
        if(this.cal.includes('sin')){this.result = Math.sin(this.cal.slice(4)*this.sys)}
        if(this.cal.includes('tan')){this.result = Math.tan(this.cal.slice(4)*this.sys)}
      }
    },
   action(item){
     //value of number
      if(!isNaN(item) || item === '.'){
        this.cal += item + ''

        //Not Repeat a '.'
        if(this.cal[this.cal.length - 1] == '.' && this.cal[this.cal.length - 2] == '.'){
          this.cal = this.cal.slice(0,-1)
        }

        /// (√)sqrt ==> this.cal [automatically]
        if(this.cal.includes('√')){
          this.result += item
          if(this.cal[0] === '√'){
          this.result = Math.sqrt(this.cal.slice(1))
         }
         /*if(this.cal[0] !== '√'){
           this.result = eval(this.cal.slice(this.cal.indexOf('√'),,))
         }*/
        }

        // Convert Repeated Operator [--] to [+] and so [---] to [-]
        if(this.cal.includes('--')){
         this.cal = this.cal.replaceAll('--','+')
       }
       if(this.cal.includes('---')){
         this.cal = this.cal.replaceAll('---','-')
       }

       // Convert (÷) to (/) But impilicty In this.result
       if(this.cal.includes('÷')){
         this.result += item
         this.result = eval(this.cal.replaceAll('÷','/'))
         //if(this.cal.includes('π')){}
       }
       // Convert (x) to (*) But impilicty In this.result
       if(this.cal.includes('×')){
         this.result += item
         this.result = eval(this.cal.replaceAll('×','*'))
       }
       /*if(this.cal.includes('÷') || this.cal.includes('×')){
         this.result = this.cal.replaceAll('÷','/')
         this.result = this.cal.replaceAll('×','*')
         this.result = eval(this.result)
       }*/
        // Convert [π] to (Math.PI)[3.141592653589793] and Mult in what before and after [π]
        if(this.cal.includes('π')){
          if(this.cal[this.cal - 1] !== 'π'){
            this.index = this.cal.indexOf('π') + 1
            this.prev = this.cal.slice(this.index,50) + 1
            this.result = eval(this.cal.replaceAll('π','*3.141592653589793*') + this.prev)
            }
         if(this.cal[0] === 'π'){
            this.result = eval(this.cal.replaceAll('π','3.141592653589793*') + item)
         }
        }

        ///Eval (this.cal) [automatically]
        if(this.cal[0] !== '√'){
          this.result += item
          if(this.cal[0] !== NaN && this.cal[0] !== 'c' && this.cal[0] !== 's' && this.cal[0] !== 't'){
            this.result = eval(this.cal)}
            }

        /// x^ ==> this.cal [automatically]
       if(this.cal.includes('^')){
         //this.result += item
         this.result = this.cal
         this.result = eval(this.result.replaceAll('^','**'))
       }

         /// Math.cos ==> this.cal [automatically]
        if(this.cal[0] === 'c' && this.cal[1] === 'o' && this.cal[2] === 's'){
          // you notice that restored value of (this.cal.slice(4)) in eval() function
          // Then multile this value in ( Math*PI*180 )
          if(this.DR === 'DEG'){
            this.result = Math.cos(eval(this.cal.slice(4))*this.sys)
            }

          if(this.DR === 'RED'){
            this.result = Math.cos(eval(this.cal.slice(4)))
            }
        }

        /// Math.sin ==> this.cal [automatically]
        if(this.cal[0] === 's' && this.cal[1] === 'i' && this.cal[2] === 'n'){
          if(this.DR === 'DEG'){
            this.result = Math.sin(eval(this.cal.slice(4))*this.sys)
            }

          if(this.DR === 'RED'){
            this.result = Math.sin(eval(this.cal.slice(4)))
            }
        }
        /// Math.tan ==> this.cal [automatically]
        if(this.cal[0] === 't' && this.cal[1] === 'a' && this.cal[2] === 'n'){
          if(this.DR === 'DEG'){
            this.result = Math.tan(eval(this.cal.slice(4))*this.sys)
            }

          if(this.DR === 'RED'){
            this.result = Math.tan(eval(this.cal.slice(4)))}
        }


      }

      if(['÷','-','×','+'].includes(item)){
        this.cal = this.cal + item
        this.result = this.result + item

        // Convert [--] to [+] And [---] to [-]
        if(this.cal.includes('--')){
          this.result = this.cal.replaceAll('--','+')
        }
        if(this.cal.includes('---')){
          this.result = this.cal.replaceAll('---','-')
        }
        // Make x not Repeate It
        if(this.cal[this.cal.length - 1] == this.cal[this.cal.length - 2]){
          this.cal = this.cal.slice(0,-1)
          this.result = this.cal
        }


      }
      if(item === 'C'){
        this.cal = ''
        this.result = ''
      }
      if(item === 'AC'){
        this.cal = this.cal.slice(0,-1)
        this.result = eval(this.cal)
        if(this.cal == ''){
          this.result = ''
        }

      }
      if(item === 'π'){
        this.cal += item
       if(this.cal[0] !== 'π'){this.result = eval(this.result * this.pi)}

       if(this.cal[this.cal.length - 1] === this.cal[this.cal.length - 2]){
        this.cal = this.cal.slice(0,-1)
         }
       if(this.cal[0] === 'π'){this.result = this.pi}

      }
      if(item === '%'){
        this.cal = this.cal / 100
        this.result = this.result /100
        }
      if(item === '√'){
        this.cal += item
        this.result = Math.sqrt(this.cal.slice(1))}

      /// x^ ==> this.cal
      if(['x^'].includes(item)){
        this.cal += '^'

       //Solve problem of duplication of '*'
        if(this.cal[this.cal.length - 1] == this.cal[this.cal.length - 2])
        {this.cal=this.cal.slice(0,-1)}

        this.result = this.cal
      }
      /// Cos,Sin,Tan
      if(item === 'COS'){this.cal = item.toLowerCase() + '(' + this.cal
      if(this.cal.includes('cos(cos('))
        {this.cal=this.cal.slice(0,-4)
        }
      }

      if(item === 'SIN'){this.cal = item.toLowerCase() + '(' + this.cal
      if(this.cal.includes('sin(sin(')){
        this.cal=this.cal.slice(0,-4)
        }
      }

      if(item === 'TAN'){this.cal = item.toLowerCase() + '(' + this.cal
      if(this.cal.includes('tan(tan('))
        {this.cal=this.cal.slice(0,-4)
        }
      }

      if(item === 'x​²'){this.cal = this.cal**2
      this.result = this.cal}
      if(item === 'x³'){this.cal = this.cal**3
      this.result = this.cal}

       ///// When Click TO [=] Mark
      if(item === '='){
        if(this.cal[0] === '√'){
          this.cal = Math.sqrt(this.cal.slice(1))
          this.result = Math.sqrt(this.cal.slice(1))
        }
        if(this.cal.includes('^')){
          this.cal = eval(this.cal.replaceAll('^','**'))  // OR this.cal = this.result
        }
        if(this.cal[0] === 'c' && this.cal[1] === 'o' && this.cal[2] === 's'){

          if(this.DR === 'DEG'){
             this.cal = Math.cos(this.cal.slice(4)*this.sys)
             this.cal = this.cal.toFixed(2)
            }

          if(this.DR === 'RED'){
             this.cal = Math.cos(this.cal.slice(4))
             this.cal = this.cal.toFixed(3)
            }
        }

        if(this.cal[0] === 's' && this.cal[1] === 'i' && this.cal[2] === 'n'){
          if(this.DR === 'DEG'){
             this.cal = Math.sin(this.cal.slice(4)*this.sys)
             this.cal = this.cal.toFixed(3)
            }

          if(this.DR === 'RED'){
             this.cal = Math.sin(this.cal.slice(4))
             this.cal = this.cal.toFixed(3)
            }
        }

        if(this.cal[0] === 't' && this.cal[1] === 'a' && this.cal[2] === 'n'){
          if(this.DR === 'DEG'){
             this.cal = Math.tan(this.cal.slice(4)*this.sys)
             this.cal = this.cal.toFixed(3)
            }

          if(this.DR === 'RED'){
             this.cal = Math.tan(this.cal.slice(4))
             this.cal = this.cal.toFixed(3)
            }
        }
        if(this.cal.includes('÷')){
          this.cal = eval(this.cal.replaceAll('÷','/'))
          // OR this.cal = this.result  OR this.cal = eval(this.result)
        }

        if(this.cal.includes('×')){
          this.cal = this.cal.replaceAll('×','*')
          // OR this.cal = this.result  OR this.cal = eval(this.result)
        }
        if(this.cal.includes('π')){
          if(this.cal[0] !== 'π'){this.cal = eval(this.result)}
          if(this.cal[this.cal - 1] !== 'π'){
            this.cal = eval(this.cal.replaceAll('π','*3.141592653589793*'))
            }
        }
        if(this.cal[0] !== '√'){
                  this.cal = eval(this.prevoperator + this.cal)
                  this.prevoperator = ''
                  this.operator = ''
        }

        if(this.cal.includes('--')){
          this.cal = eval(this.result)
        }
      }
    },

  }
}
</script>

<style lang="scss" scoped>
//variable
.center{align-items:center;text-align:center;}

 ///sass ID/CLASS
#con-calculate{
  display:flex;width:305px;height:420px;background:rgb(90, 231, 114);text-align:center;justify-content:space-between;flex-direction:column;
  .con-result{ width:280px;height:30px;outline:none;border-radius:20px;
    input{display:block;width:300px;height:20px;outline:none;border:none;background:#617f87;} //#3f9c69;
    input:nth-child(1){width:40px;height:20px;background:#617f87;color:#fff;float:right;cursor:pointer;}
    input:nth-child(2){height:45px;font-size:45px;color: #1cec1c;
    &::placeholder{color:#1cec1c;}
    }
    input:nth-child(3){color:#fff;font-size:26px;}

    }
  .con-btn{

    width:300px;line-height:220px;display:grid;grid-template-columns:repeat(4,25%);
    text-align:center;cursor:pointer;align-items:center;position:relative;

    input[type=submit]{display:grid;margin:3px 0;border:none;height:40px;width:100%;align-items:center;text-align:center;
     justify-content:center;cursor:pointer;outline:none;border-radius:50px;color:#fff;font-size:20px;
     /*background:#5ea57f;*/
     }

    // Class Of Special Buttons
    .rest-btn{background: #5ea57f;}//#6cdda1
    .dark-vue{background:#333c5a;@extend .center;}
    .blue-vue{grid-column:3/5;width:100%;@extend .center;border-radius:50px;
      background:#0e5ff3; box-shadow:0;}
    .purple-vue{background:#8218b9c7;}
    .delete-vue{background:#ef1432;}

     input[type=submit]:focus-within{border-radius: 50px;
       background: #617f87;
       box-shadow: inset 2px 2px 50p #57e58e,
            inset -7px -7px 50px #fff;}
  }

  }
</style>
