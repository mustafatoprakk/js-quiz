![Screenshot_1](https://user-images.githubusercontent.com/37051222/148634522-7fc96f68-3c8c-49e2-a1f5-84b31cf10bd4.png)



# html

```
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-8">
            <div class="card">
              <div class="card-header">Quiz</div>
              <div class="card-body">
                  <h5 id="question">?</h5>
                  <hr>
                  <div id="buttons">
                      <a href="#" id="btn0" class="btn btn-outline-dark"><span id="choice0">0</span></a>
                      <a href="#" id="btn1" class="btn btn-outline-dark"><span id="choice1">1</span></a>
                      <a href="#" id="btn2" class="btn btn-outline-dark"><span id="choice2">2</span></a>
                      <a href="#" id="btn3" class="btn btn-outline-dark"><span id="choice3">3</span></a>
                  </div>
              </div>
              <div class="card-footer" id="correct_answer"></div>
            </div>
          </div>
        </div>
    </div>
```

# js
```
function Question(text,choice,answer){
    this.text=text
    this.choice=choice
    this.answer=answer
}

Question.prototype.checkAnswer=function(answer){
    return this.answer===answer
}

// Quiz constructor
function Quiz(questions){
    this.questions=questions
    this.score=0
    this.questionIndex=0
}
// Quiz prototype - question
Quiz.prototype.getQuestion=function(){
    return this.questions[this.questionIndex]
}
// Quiz finish
Quiz.prototype.quizFinish=function(){
    return this.questionIndex===this.questions.length
}
// Quiz guess
Quiz.prototype.guess=function(answer){
    var question=this.getQuestion()
    
    if(question.checkAnswer(answer)){ // doğru ise bir arttırılacak
        this.score++
    }
    this.questionIndex++   // bir sonraki soruya geçilecek
}

var q1=new Question("Peygamber Efendimiz(a.s.v) doğum yılı?",
    ["570", "571", "581", "632"], "571")

var q2=new Question("Hangisi dört halifeden biri değildir?",
    ["Hz. Hamza", "Hz. Ali", "Hz. Ebubekir", "Hz. Osman"], "Hz. Hamza")

var q3=new Question("Fatih Sultan Mehmet İstanbulu fethettiğinde kaç yaşındaydı?",
    ["25", "41", "21", "30"], "21")

var questions=[q1,q2,q3]

// Start quiz
var quiz=new Quiz(questions)

console.log(quiz.getQuestion())


let textQuestion=document.querySelector("#question")
let correctAnswer=document.querySelector("#correct_answer")

loadQuestion()

function loadQuestion(){
    if(quiz.quizFinish()){
        // score
        showScore()
    }else{
        // show
        var question=quiz.getQuestion()
        var choice=question.choice
        textQuestion.textContent=question.text

        for(let i=0;i<choice.length;i++){
            let element=document.querySelector("#choice"+i)
            element.innerHTML=choice[i]          
            
            guess("btn"+i,choice[i])
        }

        showProgress()
    }
}

function guess(id, guess){
    var btn=document.getElementById(id)
    btn.onclick=function(){
        quiz.guess(guess)
        loadQuestion()
    }
}

function showScore(){
    var returnScore=`<h2>Score </h2><h4>${quiz.score}</h4`
    var ret=document.querySelector(".card-body").innerHTML=returnScore
}

function showProgress(){
    var totalQuestion=quiz.questions.length
    var questionNumber=quiz.questionIndex+1
    document.querySelector("#correct_answer").innerHTML="Question "+questionNumber+ " of "+totalQuestion
}



```
