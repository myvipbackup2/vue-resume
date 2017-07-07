<template>
    <div id="app">
        <p>{{code}}</p>
        <div v-html="computeStyle"></div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                code: ``,
                finalCode: `#app {width:600px;margin:0 auto;font-family: 'Avenir', Helvetica, Arial, sans-serif;-webkit-font-smoothing: antialiased;-moz-osx-font-smoothing: grayscale;text-align: center;color: #2c3e50;margin-top: 60px;background: #ccc;}`
            }
        },
        created(){
            let n = 0;
            let timer = setInterval(() => {
                if (n === this.finalCode.length) {
                    clearInterval(timer);
                    console.log(`end`);
                }
                this.code = this.finalCode.substr(0, n);
                n++;
            }, 60)
        },
        computed: {
            computeStyle(){
                return `<style>${this.code}</style>`;
            }
        }
    }
</script>

