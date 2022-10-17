<template>
    <div>
    
        <h2>당첨 숫자</h2>
        <div id="결과창">
            <lotto-ball v-for="ball in winBalls" :key="ball" v-bind:number="ball"></lotto-ball>
        </div>
        <h2>보너스</h2>
            <lotto-ball v-if="bonus" v-bind:number="bonus"></lotto-ball>
        <button v-if="redo" @click="onClickRedo">한 번 더!</button>
    </div>
</template>

<script>
    import LottoBall from './LottoBall';

    function operateGenerate(){
        const candidateBall = Array(45).fill().map((item, index)=>{ return index+1 });
        const shuffle = [];

        while(candidateBall.length > 0){
            shuffle.push(candidateBall.splice(Math.floor(Math.random() * candidateBall.length),1)[0]);
        }

        const bonusNumber = shuffle[shuffle.length - 1];
        const winNumbers = shuffle.slice(0,6).sort((a,b)=>{ return a-b; });
        console.log("winNumbers길이 : "+winNumbers);
        return [...winNumbers, bonusNumber];
    }

    const timeouts = [];

    export default {
        components : {
            'lotto-ball' : LottoBall
        },
        data(){
            return{
                winNumbers : operateGenerate(),
                winBalls : [],
                bonus : null,
                redo : false,
            }
        },
        methods : {
            onClickRedo(){
                this.winNumbers = operateGenerate();
                this.winBalls = [];
                this.bonus = null;
                this.redo = false;
                this.drawLottery();

            },
            drawLottery(){
                for(let i =0; i< this.winNumbers.length-1; i++){
                        timeouts[i] = setTimeout(()=>{
                            this.winBalls.push(this.winNumbers[i]);
                            console.log(i+1);
                        }, (i+1)*1000);
                    }
                    timeouts[this.winNumbers.length] = setTimeout(()=>{
                        this.bonus = this.winNumbers[6];
                        this.redo = true;
                }, (this.winNumbers.length+1)*1000);
                    console.log("mount");
            }
        },
        computed : {

        },
        watch : {

        },
        mounted() {
            console.log("this.winNumbers "+this.winNumbers.length);
            this.drawLottery();
        },
        beforeDestroy() {
            console.log("beforeDestroy");
            timeouts.forEach((t)=>{
                clearTimeout(t);
            });
        },
    }
</script>

<style scoped>
#결과창{
    display :flex;
}
</style>