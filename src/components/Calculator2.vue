<template>
    <section>
        <div class="calc-container">
        <!-- Calculate Result Section -->
        <div class="calculator">
            <div class="header">
                <p>Semester 2</p>
                <i class="fas fa-plus" @click="addNewCourse"></i>
            </div>

            <table>
                <tr class="table-headings">
                
                    <th>Course Name</th>
                    <th>Grade</th>
                    <th>Course Unit</th>
                </tr>
                <tr v-for="(item, index) in courseList" :key="index"> 
                    
                    <td><input type="text" v-model="item.courseName"></td>
                    <td>
                        <select ref="courseGrade" v-model="item.courseGrade">
                            <option>A</option>
                            <option>B</option>
                            <option>C</option>
                            <option>D</option>
                            <option>E</option>
                            <option>F</option>
                            <option>AR</option>
                        </select>
                    </td>
                    <td>
                        <input type="number" v-model="item.courseUnit">
                    </td>
                </tr>
            </table>            
        </div>
        <!-- End of Calculate Result Section        -->

        <!-- Result Section -->
        <div class="result">
                <div class="header">
                    <p id="title">Current GPA</p>
                </div>
                
                <!-- Result -->
                <div class="result">
                   <p id="gpa" ref="gpa">{{secondSemesterGPA}}</p>
                   <p id="class">{{sclass}}</p>
                </div>
        </div>
        <!-- End of Result Section -->
        </div>
        <div class="calc-footer">
            <button @click="clearAll">Clear All</button>
            <button @click="calculate">Calculate</button>
        </div>
    </section>
</template>

<script>

export default {
    data(){
        return{
            courseid: 0,
            courseList: [
                {
                courseName: '',
                courseGrade: '',
                courseUnit: '',
                }
            ],
            point: 0,
            scores: [],
            sumOfCoursesUnit: 0,
            secondSemesterGPA: 0,
            sclass: '',
        }
    },
    methods:{
        addNewCourse(){
            this.courseList.push({
                courseName: '',
                courseGrade: '',
                courseUnit: '',
            })
        },
        getCourseScore(){
            // Looping through the courseList to get the score of each courses
            for(let i = 0; i < this.courseList.length; i++){
                if(this.courseList[i].courseGrade === 'A'){
                    this.point = 5
                }else if(this.courseList[i].courseGrade === 'B'){
                    this.point = 4
                }else if(this.courseList[i].courseGrade === 'C'){
                    this.point = 3
                }else if(this.courseList[i].courseGrade === 'D'){
                    this.point = 2
                }else if(this.courseList[i].courseGrade === 'E'){
                    this.point = 1
                }else{
                    this.point = 0
                }
                let score = this.courseList[i].courseUnit * this.point
                this.scores.push(score)
                this.sumOfCoursesUnit+= this.courseList[i].courseUnit
            }
        },
        getStudentClass(){
            // To get class
            if(this.secondSemesterGPA > 4.49){
                this.sclass = "First Class"
                this.$refs.gpa.style.color = 'green'
            }else if(this.secondSemesterGPA > 3.49 && this.secondSemesterGPA < 4.50){
                this.sclass = " Second Class Upper"
                this.$refs.gpa.style.color = 'lightgreen'
            }else if(this.secondSemesterGPA> 2.49 && this.secondSemesterGPA < 3.50){
                this.sclass = "Second class Lower"
                this.$refs.gpa.style.color = 'yellow'
            }else if(this.secondSemesterGPA < 2.50 && this.secondSemesterGPA >= 2.00){
                this.sclass = "Third Class, You Can Do More"
                this.$refs.gpa.style.color = 'crimson'
            }else if(this.secondSemesterGPA < 2.00){
                this.sclass = "Don't be sad, You can do it!!!"
                this.$refs.gpa.style.color = 'red'
            }else if(this.secondSemesterGPA === null || isNaN(this.secondSemesterGPA)){
                alert('Fill In Your Courses')
                this.secondSemesterGPA = 0
            }
        },
        calculate(){ 
            this.getCourseScore()
            
            // Calculating the scores of all courses
            let sumOfScores = 0
            for(let i = 0; i <this.scores.length; i++){
               sumOfScores+= this.scores[i]
            }

            // Calculating the secondSemesterGPA by dividing the scores of all courses by the sum of all courses unit
            this.secondSemesterGPA = sumOfScores / this.sumOfCoursesUnit
            this.secondSemesterGPA = this.secondSemesterGPA.toFixed(2)

            this.getStudentClass()  
            this.$emit('secondSemesterGpa', this.secondSemesterGPA)
        },
        clearAll(){
            this.courseList = [
                {
                courseName: '',
                courseGrade: '',
                courseUnit: '',
                }
            ];
            this.sclass = ""
            this.secondSemesterGPA = 0
            this.$refs.gpa.style.color = ''
        }
    }
}
</script>

<style scoped>

section{
    width: 100%;
    margin: 30px 0;
    box-shadow: 0px 0px 3px;
    background: #fff;
}
.calc-container{  
    display: grid;
    grid-template-columns: 70% 30%;
}

section .calc-footer{
    background-color: #f5f6f8;
    padding: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

section .calc-footer button{
    background-color: #d62181;
    box-shadow: 0px 0px 4px;
    color: #fff;
    font-weight: bolder;
    outline: none;
    border: none;
    border-radius: 10px;
    padding: 10px;
    cursor: pointer;
}

.calc-container .calculator{
    border-right: 1px #eee solid;
}

section .calc-container .calculator table{
    width: 90%;
    margin: 10px auto;
    border:none;
}

section .calc-container .calculator table .table-headings{
    background: #fff;
}

section .calc-container .calculator table th{
    border:none;
}

section .calc-container .calculator table tr{
    background: #f5f6f8;
}

section .calc-container .calculator table tr td{
    padding: 10px 8px 10px 8px;
    border: none;
}

section .calc-container .calculator table tr td input{
    text-align: center;
}

.calc-container .result{
    text-align: left;
}

.calc-container .calculator .header, .calc-container .result .header{
    border-bottom: 1px solid #eee;
}

.calc-container .calculator .header{
    width: 90%;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.calc-container .calculator .header .fas{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    background-color: #e7f8e8;
    border-radius: 50%;
}

.calc-container .calculator .header p, .calc-container .result .header p{
    text-align: left;
    text-transform: uppercase;
    color: #2c3e50;
}


.calc-container .result #title{
    width: 90%;
    margin: 20px auto;
    color: #7a828b;
}

.calc-container .result .result{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.calc-container .result .result #gpa{
    background-color: #fff;
    font-size: 30px;
    width: 100px;
    height: 100px;
    box-shadow: 0px 0px 5px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

@media (max-width:800px){
    .calc-container{  
    display: grid;
    grid-template-columns: 1fr;
}
}

@media (max-width:600px){

    section .calc-container .calculator table tr{
        width: 100%;
    }
    section .calc-container .calculator table th{
        font-size: 15px;
    }
    section .calc-container .calculator table tr td{
        border: none;
    }
    section .calc-container .calculator table tr td input{
        width: 100%;
    }
}

</style>
