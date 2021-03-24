<template>
    <div class="audio-btn">
        <audio ref="bgm" id="audioPlayer" autoplay loop>
            <source src="../assets/music/Upon Serendipity.mp3" type="audio/mp3">
        </audio>
        <button v-on:click="toggleAudio()">Toggle BGM</button>
    </div>
</template>

<script>
export default {
    name: 'Audio',
    
    data() {
        return{

        }
    },

    props: {
        // gachaList: {
        //     type: Array,
        // },
    },
    
    methods: {
        toggleAudio: function() {
            var audio = this.$refs.bgm;

            if(audio.paused){
                audio.play();
            }
            else{
                audio.pause();
                audio.currentTime = 0;
            }
        },
    },

    mounted() {
        this.$refs.bgm.volume = 0.10;
    },

}

</script>

<style>

.audio-btn {
  margin: 3px;
}

.audio-btn > button {
  width: 100px;
  height: 30px;
}

</style>