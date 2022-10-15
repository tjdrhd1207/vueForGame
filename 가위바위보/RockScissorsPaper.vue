<template>
    <div>
        <div id="computer" :style="computedStyleObject"></div>
        <div>
            <button v-on:click="onClickButton('rock')">바위</button>
            <button v-on:click="onClickButton('scissors')">가위</button>
            <button v-on:click="onClickButton('paper')">보</button>
        </div>
        <div>{{result}}</div>
        <div>현재 {{score}} 점</div>
    </div>
</template>

<script>
    const rspCoords = {
        rock : '0px',
        scissors : '-142px',
        paper : '-284px',
    };

    const scores = {
        scissors : 1,
        rock : 0,
        paper : -1,
    };

    const computerChoice = (imgCoord) => {
        return Object.entries(rspCoords).find(function(v){
            return v[1] === imgCoord;
        })[0];
    };

    let interval = null;
    export default {
        data(){
            return{
                imgCoord : rspCoords.scissors,
                result : '',
                score : 0,
            }
        },
        computed : {
            computedStyleObject(){
                return {
                    background : `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`,
                }
            }
        },
        methods : {
            changeHand(){
                
                    interval = setInterval(()=>{
                        if(this.imgCoord == rspCoords.scissors){
                            this.imgCoord = rspCoords.paper;
                        }else if(this.imgCoord == rspCoords.paper){
                            this.imgCoord = rspCoords.rock;
                        }else if(this.imgCoord == rspCoords.rock){
                            this.imgCoord = rspCoords.scissors;
                        }
                    }, 100);
                
            },
            async onClickButton(choice){
                clearInterval(interval);
                const myScore = scores[choice];
                const cpuScore = scores[computerChoice(this.imgCoord)];
                const diff = myScore -cpuScore;
                console.log("choice : "+choice+", myscore : "+myScore+", cpuScore : "+cpuScore+", diff : "+diff);

                if(diff === 0 ){
                    this.result = '비김';
                }else if([-1, 2].includes(diff)){
                    this.result = '이김';
                    this.score += 1;
                }else if([1, -2].includes(diff)){
                    this.result = '짐';
                    this.score -= 1;
                }

                setTimeout(()=>{
                    this.changeHand();
                },3000);
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
                    this.changeHand();
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
    #computer{
        width : 142px;
        height : 200px;
        background-position : 0 0;
    }
</style>