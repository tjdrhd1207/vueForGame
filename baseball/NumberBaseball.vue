<template>
    <div>
        <h1>{{result}}</h1>
        <form @submit.prevent="onSubmitForm">
    
            <input ref="answer" minlength="4" maxlength="4" v-model="value" />
            
            <button type="submit">입력</button>
        </form>
        <div>시도 : {{tries.length}}</div> 
        <ul>
            <li v-for="(item, index) in tries" :key="index">{{item}}</li>
        </ul> 
       
    </div>
</template>

<script>

    const getRandomNumber = () =>{
        const randomNum = Math.ceil(Math.random()*9000)+1000;
        const array = [];
        
        for(let i = 0; i < randomNum.toString().length; i++){
            array.push(parseInt(randomNum.toString().substring(i,i+1)));
        }
        console.log("array : "+array+", randomNum : "+randomNum+", randSize : "+randomNum.toString().length);
        return array;
    }

    export default {
        data(){
            return {
                tries : [],
                value : '',
                result : '',
                correctAnswer : getRandomNumber(),

            }
        },
        methods : {
            onSubmitForm(e){
                //e.preventDefault();
                console.log(this.value.length);

                if( this.value === this.correctAnswer.join('')){                //정답 맞췄으면
                    this.tries.push({
                        try : this.value,
                        result : '홈런',
                    });
                    this.result = '홈런';
                    alert('게임을 다시 실행합니다.');
                    this.value = '';
                    this.tries = [];
                    this.$refs.answer.focus();
                } else {

                    if(this.tries.length == 9){                                 //10번째 시도
                        alert('게임을 다시 실행합니다.');
                        this.result = `10번 넘게 틀려서 실패! 답은 ${this.correctAnswer.join(',')}`;
                        this.value = '';
                        this.correctAnswer = getRandomNumber();
                        this.tries = [];
                        this.$refs.answer.focus();
                        
                    }else{
                        let strike = 0;
                        let ball = 0;
                        const answerArray = this.value.split('').map(v => parseInt(v));
                        console.log("answerArr : "+answerArray);
                        
                        for(let i=0; i<answerArray.length; i++){
                            if(answerArray[i] == this.correctAnswer[i]){            //숫자 자릿수 모두 같을 때
                                strike++;
                            }else if(this.correctAnswer.includes(answerArray[i])    ){ //숫자만 같을 때
                                ball++;
                            }                                      
                        }
                        this.tries.push({
                            try : this.value,
                            result : `${strike} 스트라이크, ${ball} 볼`,
                        });
                        this.value = '';
                    }
                }

                console.log(this.strike +" , "+this.ball);
                this.valueObject = [];

                
                
                
            }
        }
    }
</script>

<style lang="">
    
</style>