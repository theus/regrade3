<template>
  <div class="Regrade3">
    <main class="uk-flex uk-flex-center Regrade3-main">
      <div class="uk-flex-left">
        <div><input type="text" v-model="vars.a" tabindex="1" @keyup="validate('a')" placeholder="10" class="uk-form-blank Regrade3-input"></div>
        <div><input type="text" v-model="vars.c" tabindex="3" @keyup="validate('c')" placeholder="x" class="uk-form-blank Regrade3-input"></div>
      </div><!-- /.uk-flex-left -->

        <div class="uk-flex-center" v-if="invertido">
          <?xml version="1.0" encoding="utf-8"?>
          <!-- Generator: Adobe Illustrator 20.0.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
          <svg version="1.1" class="Regrade3-rule" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 92 46" style="enable-background:new 0 0 92 46;" xml:space="preserve">
            <g>
              <g>
                <path d="M10.3,9.3c23.8,0,47.6,0,71.3,0c1.6,0,1.6-2.5,0-2.5c-23.8,0-47.6,0-71.3,0C8.7,6.8,8.7,9.3,10.3,9.3L10.3,9.3z"/>
              </g>
            </g>
            <g>
              <g>
                <path d="M10.3,40.3c23.8,0,47.6,0,71.3,0c1.6,0,1.6-2.5,0-2.5c-23.8,0-47.6,0-71.3,0C8.7,37.8,8.7,40.3,10.3,40.3L10.3,40.3z"/>
              </g>
            </g>
          </svg>
        </div>
        <div v-else>
          <?xml version="1.0" encoding="utf-8"?>
          <!-- Generator: Adobe Illustrator 20.0.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
          <svg version="1.1" class="Regrade3-rule" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 92 46" style="enable-background:new 0 0 92 46;" xml:space="preserve">
            <g>
              <g>
                <path d="M10.2,39.9c24-10.1,48-20.3,72.1-30.4c1.5-0.6,0.8-3-0.7-2.4c-24,10.1-48,20.3-72.1,30.4C8.1,38.1,8.7,40.6,10.2,39.9
                L10.2,39.9z"/>
              </g>
            </g>
            <g>
              <g>
                <path d="M81.3,37.5c-24-10.1-48-20.3-72.1-30.4C7.7,6.5,7.1,8.9,8.5,9.5c24,10.1,48,20.3,72.1,30.4C82.1,40.6,82.7,38.1,81.3,37.5
                L81.3,37.5z"/>
              </g>
            </g>
          </svg>
        </div>

      <div class="uk-flex-right">
        <div><input type="text" v-model="vars.b" tabindex="2" @keyup="validate('b')" placeholder="7" class="uk-form-blank Regrade3-input"></div>
        <div><input type="text" v-model="vars.d" tabindex="4" @keyup="validate('d')" placeholder="8" class="uk-form-blank Regrade3-input"></div>
      </div><!-- /.uk-flex-right -->
    </main>
    <form class="uk-form Regrade3-form">
      <label>
        <input class="" type="checkbox" v-model="invertido" @change="result()">
          Inversamente proporcional
      </label>
    </form>
    <hr class="Regrade3-hr">
    <div class="Regrade3-result">
      <p class="uk-text-muted uk-text-center">Resultado:</p>
      {{getResult}}
    </div><!-- /.Regrade3-result -->
  </div><!-- /.Regrade3 -->
</template>

<script>
const svg = require('../assets/3-rule.svg')
export default {
  data () {
    return {
      // note: changing this line won't causes changes
      // with hot-reload because the reloaded component
      // preserves its current state and we are modifying
      // its initial state.
      invertido: false,
      getResult: '',
      vars: {a:'', b:'', c:'', d:''}
    }
  },
  methods: {
    validate: function(variable){
      let vars = this.vars
      let data = this.vars[variable]

      // only x and numbers are allowed
      if(data !== "x") {
        this.vars[variable] = data.replace(/([^^0-9]+)/, '')
      }

      // just one x at all program
      var qtdOfX = 0
      for (let key in vars) {
        if(qtdOfX > 0 && vars[key] == 'x') vars[key] = ''
        if (vars[key] == 'x') qtdOfX++
      }

      this.result()
    },
    result: function() {
      var vm = this
      function rule3(directVar1, directVar2, otherVar1, otherVar2) {
        if (typeof parseInt(directVar1) === "number" && typeof parseInt(directVar2) === "number") {
          let result = directVar1*directVar2
          if (otherVar1 === "x") vm.getResult = `x = ${(result / otherVar2).toFixed(2).toString().replace('.', ',')}`
          if (otherVar2 === "x") vm.getResult = `x = ${(result / otherVar1).toFixed(2).toString().replace('.', ',')}`
        }

        if (typeof parseInt(otherVar1) === "number" && typeof parseInt(otherVar2) === "number") {
          let result = otherVar1*otherVar2
          if (directVar1 === "x") vm.getResult = `x = ${(result / directVar2).toFixed(2).toString().replace('.', ',')}`
          if (directVar2 === "x") vm.getResult = `x = ${(result / directVar1).toFixed(2).toString().replace('.', ',')}`
        }
      }
      let vars = this.vars
      var {a,b,c,d} = vars
      if (a !=="" && b !=="" && c !=="" && d !=="") {
        if (!this.invertido) rule3(a,d,b,c)
        if ( this.invertido) rule3(a,b,c,d)
      }
    }
  }
}
</script>

<style lang='sass?outputStyle=compressed'>
$regrade3-bgcolor: #F5F5F5;

.Regrade3 {
  background-color: $regrade3-bgcolor;
  width: 500px;
  display: block;
  margin: 0 auto;
  border-radius: 5px;

  @media screen and (max-width: 768px) {
    width: 90%;
  }

  &-main { padding-top: 30px; }

  &-rule {
    fill: darken($regrade3-bgcolor, 30%);
    margin-top: -20px;
    width: 92px;
    height: 92px;
  }

  &-input {
    background-color: darken($regrade3-bgcolor, 5%)!important;
    width: 60px;
    margin-bottom: 10px;
  }

  &-result {
    text-align: center;
    padding-bottom: 10px;
  }

  &-form {
    text-align: center;
    /* padding-bottom: 15px; */

    label:hover { cursor: pointer; }
  }
}
</style>
