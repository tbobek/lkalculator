<template>
    <h1>LK-Berechnung</h1>
    <div>
        <p>
            <label for="lkWinnerInput">LK Gewinner: </label>
            <input type="number" size="70" step="0.1" max="25" min="1" id="lkWinnerInput" v-model.number="lkWinner">
        </p>
        <p>
            <label for="lkLooserInput">LK Verlierer: </label>
            <input type="number" size="80" step="0.1" max="25" min="1"  id="lkLooserInput" v-model.number="lkLooser">
        </p>
        <p>
            <label for="ageClassInput">Altersklasse: </label>
            <input type="number" size="50" step="10" max="50" min="40"  id="ageClassInput" v-model.number="ageClass">
    
        </p>
      
      <h2><b>LK-Verbesserung: {{ lkGain.toFixed(3) }}</b></h2>
    </div>
    <p><a href="https://www.dtb-tennis.de/Verband/Der-DTB/Regeln-Ordnungen" target="_blank">LK Informationen auf der DTB Website</a> siehe Link dort: "Durchführungsbestimmungen zur Leistungsklassenordnung"
    </p>
  </template>
  
  <script>
  export default {
    name : "LkRechner", 
    data() {
      return {
        lkWinner: 22.0, 
        lkLooser: 22.0, 
        ageClass: 50, 
        lkGain: this.calcGain()
      }
    },
    watch: {
        lkWinner() {
            this.lkGain = this.calcGain()
        }, 
        lkLooser() {
            this.lkGain = this.calcGain()
        }, 
        ageClass() {
            this.lkGain = this.calcGain()
        }
    },
    methods: {
      calcGain() {
        // Here you can add your code to execute the queries based on the number of queries entered
        let d = parseFloat(this.lkWinner) - parseFloat(this.lkLooser)
        let h = 10 * (30 - Math.floor(parseFloat(this.lkWinner)))
        let a = 0.7
        let p = 110
        
        if (this.ageClass != 50 ) {
            a = 0.8 // LK40
        }
        
        if(d<-4) {
            p = 10
        }
        if(d>=-4 && d<0) {
            p = -1.25 * d * d * d - 7.5 * d * d + 50
        }
        if(d>=0 && d<4) {
            p = -1.875 * d * d * d + 11.25 * d * d + 50 
        }
        if(d>=4) {
            p = 110
        }
        console.log("d=" + d + ", p=" + p + ", h=" + h + ", a=" + a)
        return p/h * a 
      },
    }
  }
  </script>