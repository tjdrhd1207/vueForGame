<template>
    <div>
        <div class="ball" v-bind:style="styleObject">
            <span class=""></span>
            <span class="num">{{number}}</span>
        </div>
    </div>
</template>

<script>

    export default {
        name : 'LottoBall',
        props : {
            number : Number,
        },
        computed : {
          styleObject(){
            let background;
            if(this.number <= 10){
                background =  'radial-gradient(circle at 50px 20px, red, black)';
            }else if(this.number <= 20){
                background =  'radial-gradient(circle at 50px 20px, orange, black)';
            }else if(this.number <= 30){
                background =  'radial-gradient(circle at 50px 20px, yellow, black)';
            }else if(this.number <= 40){
                background =  'radial-gradient(circle at 50px 20px, green, black)';
            }else {
                background =  'radial-gradient(circle at 50px 20px, blue, black)';
            }
            return {background}

          }
        },
        data(){
            return{
                
            }
        },
        methods : {
           
        }
    };

</script>

<style scoped>
    .ball{
        display: inline-block;
        width: 100px;
        height: 100px;
        margin: 0;
        border-radius: 50%;
        position: relative;
        /* background : radial-gradient(circle at 50px 20px, #c95d54, black); */
        animation-name : appear;
        animation-duration : 5s;
        animation-iteration-count : infinite;
        animation-play-state: paused;
        text-align: center;
        line-height: 100px;
        font-size: 40px;
        color: white;
        font-family : fantasy;
        margin-right : 20px;
        
        /* animation-name: appear;
        animation-duration: 3s;
        animation-play-state: running; 
        animation-iteration-count: 2; */

        animation-name : bounce;
        animation-duration: 3s;
        animation-play-state: running; 
        animation-iteration-count : 2;
        
    }

       /* @keyframes rule */
       @keyframes appear {
            0% {left : 0px;}
            50% {left : 150px; }
            100% {left : 0px; }
        }

        @keyframes bounce {
            0%, 100%{
                bottom : 0px;
                animation-timing-function : ease-out;
            }

            50%{
                bottom : 100px;
                animation-timing-function : ease-in;
            }
        }

</style>