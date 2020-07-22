<template>
    <v-card class='mt-8'>
        <v-card-title class='text-center justify-center py-6'>
            <h1 class='font-weight-bold display-3 title'>Pomodoro Clock</h1>    
        </v-card-title>
        <v-tabs
            v-model="timerType"
            background-color="transparent"
            color="dark"
            grow
            @change="changeCurrentTimer"
        >
            <v-tab
                v-for="tab in timers"
                :key="tab">
                {{tab.name}}
            </v-tab>

            <v-tabs-items v-model="timerType">
                <v-tab-item>

                    <v-card flat class='pa-5'>
                        <h1 class='text-center py-6' id='clockDigits'>{{displayMintues}}:{{displaySeconds}}</h1>
                        <div class='btn-group'>    
                            <v-btn outlined @click="start">
                                <v-icon left small>mdi-play-circle-outline</v-icon>
                                Start</v-btn>
                            <v-btn outlined @click="stop">
                                <v-icon left small>mdi-stop-circle-outline</v-icon>
                                Stop</v-btn>
                            <v-btn outlined @click="reset(timers[currentTimer].minutes)" :disabled="isRunning">
                                <v-icon left small>mdi-restart</v-icon>
                                Reset</v-btn>
                        </div>
                    </v-card>

                </v-tab-item>
            </v-tabs-items>

        </v-tabs>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            timerInstance: null,
            isRunning: false,
            totalSeconds: 25 * 60,
            currentTimer: 0,
            timers: [
                {
                    name: 'Pomodoro',
                    minutes: 25
                },
                {
                    name: 'Short Break',
                    mintues: 5
                },
                {
                    name: 'Long Break',
                    mintues: 10
                }
            ]
        }
    },
    computed:{
        displayMintues(){
            const minutes = Math.floor(this.totalSeconds / 60);
            return this.formatTime(minutes); 
        },
        displaySeconds(){
            const seconds = this.totalSeconds % 60;
            return this.formatTime(seconds);
        }
    },
    methods: {
        formatTime(time){
            if(time < 10){
                return '0' + time;
            }
            return time.toString();
        },
        start(){
            this.stop();
            this.isRunning = true;
            this.timerInstance = setInterval(() => {
                this.totalSeconds -= 1;
            }, 1000);
        },
        stop(){
            this.isRunning = false;
            clearInterval(this.timerInstance);
        },
        reset(minutes){
            this.stop();
            this.totalSeconds = minutes * 60;
        },
        changeCurrentTimer(num){
            this.currentTimer =  num;
            this.reset(this.timers[num].minutes);
        }
    }
}


</script>

<style lang='sass' scoped>
.v-card
  width:600px
.btn-group
  display: flex
  justify-content: space-around
h1
  font-size:4em
        
</style>