<template>
    <div class="alert alert-primary">
        <h3>{{ x }} + {{ y }} = ?</h3>
        <hr>
        <div class="buttons">
            <button class="btn btn-lg btn-primary"
                v-for="number in answers"
                @click="onAnswer(number)"
            >
                {{ number }}
            </button>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['settings'],
        data() {
            return {
                x: mtRand(this.settings.from, this.settings.to),
                y: mtRand(this.settings.from, this.settings.to)
            }
        },
        computed: {
            answers() {
                let good = this.x + this.y;
                let res = [this.x + this.y];

                while(res.length < this.settings.variants) {
                    let num = mtRand(good - this.settings.range, good + this.settings.range);

                    if(res.indexOf(num) === -1) { //если не содержит такого числа
                        res.push(num);
                    }
                }
                //случайная сортировка массива
                return res.sort(() => {
                    return Math.random() > 0.5;
                });
            }
        },
        methods: {
            onAnswer(num) {
                if(num == this.x + this.y){
                    this.$emit('success');
                }
                else{
                    this.$emit('error', `${this.x} + ${this.y} = ${this.x + this.y}`)
                }
            }
        }
    }

    let mtRand = (min, max) => {
        let diff = max - min;
        return Math.floor(Math.random() * (diff + 1) + min);
    }
</script>

<style lang="scss">
.buttons{
    display: flex;
    justify-content: space-between;
}
.btn{
    margin: 10px 30px;
}
</style>