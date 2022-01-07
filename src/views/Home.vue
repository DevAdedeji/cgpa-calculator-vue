<template>
  <div class="container">
      <header>

      <div class="header-text">
        <h1>Its Calculate Your Grade Pointe Average</h1>
        <p>Calculate your high school, college, and cumulative cgpa</p>
        <div></div>
      </div>

      
      <div class="cgpa-title">
        <div class="inner">
          <h2>Your CGPA</h2>
        </div>
      </div>
      <p id="resAbt">Calculate your high school, college, and cumulative cgpa</p>


    </header>

    <Calculator @firstSemesterGpa="firstgpa" />
    <Calculator2 @secondSemesterGpa="secondgpa" />

    <button class="cgpa" @click="getCGPA">Click To Get Your Cumulative GPA</button>
  </div>
</template>

<script>
import Calculator from '../components/Calculator.vue'
import Calculator2 from '../components/Calculator2.vue'

export default {
  name: 'Home',
  data(){
    return{
      firstGPA: 0,
      secondGPA: 0,
      CGPA: 0,
      StudentClass: '',
    }
  },
  components: {Calculator, Calculator2},
  methods:{
    firstgpa(gpa){
      this.firstGPA = gpa
    },
    secondgpa(gpa){
      this.secondGPA = gpa
    },
    getCGPA(){
      this.CGPA = (Number(this.firstGPA) + Number(this.secondGPA)) / 2
      this.CGPA = this.CGPA.toFixed(2)
      if(this.CGPA > 4.49){
        this.StudentClass = "First Class"
      }else if(this.CGPA > 3.49 && this.CGPA < 4.50){
        this.StudentClass = " Second Class Upper"
      }else if(this.CGPA> 2.49 && this.CGPA < 3.50){
        this.StudentClass = "Second class Lower"
      }else if(this.CGPA < 2.50 && this.CGPA > 2.00){
        this.StudentClass = "Third Class, You Can Do More"         
      }else if(this.CGPA < 2.00){
        this.StudentClass = "Don't be sad, You can do it!!!"
      }
      if(this.CGPA === null || this.CGPA === '0.00'){
        alert('Fill in both semester courses')
      }else{
        alert(`Your Cumulative GPA is ${this.CGPA}; ${this.StudentClass}`);
      }
    }
  }
}
</script>

<style scoped>
*{
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
.container{
  width: 90%;
  margin:0 auto;
  position: relative;
}

.container header{
  display: grid;
  grid-template-columns: 70% 30%;
}

.container header #resAbt{
  display:none;
}

.container header .header-text{
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 0;
}
.container header .header-text h1{
  text-align: left;
  font-size: 60px;
  padding: 10px 0;
}
.container header .header-text p{
  padding:10px 0;
}
.container header .header-text div{
  background: #06e776;
  width: 100px;
  height: 10px;
}



.container header .cgpa-title{
  background: linear-gradient(#6a30cc, #d62181);
  display: flex;
  align-items: center;
  justify-content: center;
  border-top-left-radius: 90px;
  border-bottom-right-radius: 90px;
  position: relative;
}

.container header .cgpa-title .inner{
  background: linear-gradient(#6a30cc, #d62181);
  height: 100%;
  width: 99%;
  display: flex;
  align-items: center;
  justify-content: center;
  border-top-left-radius: 100px;
  border-bottom-right-radius: 100px;
  border: 10px #fff solid;
}
.container header .cgpa-title h2{
  color: #fff;
  font-size: 50px;
}

.container .cgpa{
  border: none;
  outline: none;
  background: linear-gradient(#6a30cc, #d62181);
  color:#fff;
  font-weight:bolder;
  padding: 10px;
  border-radius:10px;
  cursor: pointer;
}

.cgpa-container{
  position: absolute;
  top: 0;
  left:0;
  right:0;
  width:100%;
  height:100vh;
  background: rgba(0, 0, 0, 0.7);
}

@media (max-width:850px){
  .container header{
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px;
}
.container header #resAbt{
  display:block;
}
.container header .header-text{
  display: none;
}
.container header .cgpa-title{
  width: 100%;
  padding: 20px;
}

.container header .cgpa-title .inner{
  padding: 40px;
}

.container header .cgpa-title h2{
  color: #fff;
  font-size: 35px;
}
}
</style>
