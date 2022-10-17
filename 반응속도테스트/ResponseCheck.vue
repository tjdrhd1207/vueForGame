<template>
    <div>
        <div id="screen" v-bind:class="state" @click="onClickScreen">{{message}}</div>
        <template v-show="result.length">
            <div>평균시간: {{average}}</div>
            <button @click="onReset">리셋</button>
        </template>
    </div>
</template>

<script>
    let start = 0;
    let end = 0;
    let timeout = null;
    export default {
        data(){
            return {
                result : [],
                state : 'waiting',
                message : '클릭해서 시작하세요.',
            }
        },
        computed : {
            average(){
                return this.result.reduce((a, c)=> a+c , 0) / this.result.length || 0;
            }
        },
        methods : {
            onReset(){
                this.result = [];
            },
            onClickScreen(){
                
                if( this.state === 'waiting'){
                    
                    this.state = 'ready';
                    this.message = '빨간색이 되면 클릭하세요';
                    timeout = setTimeout(()=>{
                        start = new Date();
                        this.state = 'now';
                        this.message = '클릭!';
                    }, Math.floor(Math.random())*1000 + 2000);
                }else if(this.state === 'ready'){
                    clearTimeout(timeout);
                    this.state = 'waiting';
                    this.message = '너무 빨리 클릭하셨어요';
                }
                else if(this.state = 'now'){
                    end = new Date();
                    this.state = 'waiting';
                    this.result.push(end-start);
                    this.message = `클릭해서 시작하세요 이전기록`;
                    
                    
                }
            },
            
        }
    };
</script>

<style scoped>
    #screen {
        width : 300px;
        height :200px;
        text-align : center;
        user-select : none;
    }
    #screen.waiting {
        background-color : aqua;
    }
    #screen.ready {
        background-color : green;
    }
    #screen.now {
        background : red;
    }
</style>