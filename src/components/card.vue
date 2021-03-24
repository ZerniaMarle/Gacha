<template>
    <div class="card-container">
        <div class="card" v-for="n in cardList.gacha.length" :key="n"> <!-- 반복 -->
            <div class="card-inner" :class="{ flipedCard : flipFlag[n-1] ? true : false }" @click="clickedCard(n-1)">
                <div class="card-front">
                    <img :src="require(`../assets/artifact/card_back.png`)" width="140px" height="224px">
                </div>

                <div class="card-back">
                    <img :src="require(`../assets/artifact/card_${cardList.gacha[n-1]}.png`)" width="140px" height="224px">
                </div>
            </div>
        </div>
    </div>
</template>

<script scoped>
export default {
    name: 'Card',

    data() {
        return {
            flipFlag: [],
        }
    },

    props: {
        cardList: {
            type: Object,
            required: true,
        },
    },

    methods: {
        clickedCard: function(index) {
            this.incCount(this.cardList.grade[index]);
            this.$set(this.flipFlag, index, true);
        },

        incCount: function(grade){
            this.$emit('incCount', grade);
        },
    },

    watch: {
        cardList: {
            deep: true,
            handler(){
                this.flipFlag = [];
                for(var i=0;i<this.cardList.gacha.length;i++){
                    if(this.cardList.grade[i] < 2)
                        this.flipFlag.splice(this.flipFlag.length, 0, false);
                    else
                        this.flipFlag.splice(this.flipFlag.length, 0, true);
                }
            },
        },
    },

}
</script>

<style scoped>

.card-container {
    width: 800px;
    min-height: 468px;
    flex-direction: row;
    justify-content: center;
    align-content: center;
    flex-wrap: wrap;
}

.card {
    background-color: transparent;
    width: 140px;
    height: 224px;
    border: none;
    perspective: 1000px;
    margin: 5px;
}

.card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    align-content: center;
    transition: transform 0.3s;
    transform-style: preserve-3d;
}

.flipedCard {
    transition-delay: 0.1s;
    transform: rotateY(180deg);
}

.card-front, .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden; /* Safari */
    backface-visibility: hidden;
}

.card-back{
    background-color: transparent;
    transform: rotateY(180deg);
}


</style>