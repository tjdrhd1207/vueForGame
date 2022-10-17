<template>
    <div>
        <div id="frame" :style="computedStyledObject"></div>

        <div>
            <button v-on:click="onClickButton('rock')">바위</button>
            <button v-on:click="onClickButton('scissors')">가위</button>
            <button v-on:click="onClickButton('paper')">보</button>
        </div>
        <div>결과 : {{result}}</div>
        <div>점수 : {{count}}</div>
    </div>
</template>


<script>
    
    const nowState = {
                rock : 0,
                scissors : -140,
                paper : -280,
            };
    const score = {
        rock : 1,
        scissors : 0,
        paper : -1,
    };

    const computerBindData = (nowInput) => {
        return Object.entries(nowState).find(function(now){
            return now[1] === nowInput;
        })[0];
    };

    let interval = null;
    export default {
        data : {
            changeData : nowState.rock,
            result : '',
            count : 0,
        },
        computed : {
            computedStyledObject(){
                return {
                    background : `url('https://en.pimg.jp/023/182/267/1/23182267.jpg') ${this.changeData}px 0px`,
                }
            }
        },
        methods :{
            changeImg(){
                console.log("체인지");
                
                interval = setInterval(()=>{
                    if(this.changeData == nowState.paper){
                        this.changeData = nowState.rock;
                    }else if(this.changeData == nowState.rock){
                        this.changeData = nowState.scissors;
                    }else if(this.changeData == nowState.scissors){
                        this.changeData = nowState.paper;
                    }
                }, 100);
                
            },
                onClickButton(inputData){
                    
                    /* for(const [key,value] of Object.entries(nowState)){
                        console.log(` ${key} : ${value}`);
                    } */

                    const userChoice = score[inputData];
                    const cpuChoice = score[computerBindData(this.changeData)];
                    console.log("유저의 선택 : "+userChoice+", 컴퓨터의 선택 : "+cpuChoice);

                    const diff = userChoice - cpuChoice;
                    console.log("diff : "+diff);

                    if(diff === 0){
                        return this.result = '비김';
                    }else if([1,-2].includes(diff)){
                        this.count++;
                        return this.result = '이김';
                    }else if([-1,2].includes(diff)){
                        this.count--;
                        return this.result = '짐';
                    }
                    

                    clearInterval(interval);
                    

                    setTimeout(()=>{
                        this.changeImg();
                    }, 3000);
                },
            },
            beforeCreate(){
            console.log('beforeCreated');
            },
            created(){
                    console.log('created');
                },
            beforeMount(){
                console.log('beforeMounted');
                
            },    
            mounted(){
                    console.log('mounted');
                    setTimeout(()=>{
                        this.changeImg();
                    },1000);

            },
            beforeUpdated(){
                console.log('beforeUpdated');
            },    
            updated(){
                    console.log('updated');
            },
            beforeDestroyed(){
                console.log('beforeDestroyed');
                clearInterval(interval);
            },
            destroyed(){
                    console.log('destroyed');
            },
            
        
        
    };

</script>


<style scoped>
#frame{
    background : url('https://en.pimg.jp/023/182/267/1/23182267.jpg');
    width : 140px;
    height: 200px;
}

</style>
