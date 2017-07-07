<template>
    <div id="app" class="styleEditor">
        <pre>{{code}}</pre>
        <div v-html="computeStyle"></div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                code: ``,
                finalCode: `
/*
* http://www.lizixiang.cn/
* 大家好，我是李子翔
* 7月了，好多公司都在招实习，你是不是也在准备简历呀。
* 金九银十，错过了暑期实习，要抓住校招的机会！
* 我们也来写一份酷炫的简历！
* 这次使用的技术栈主要是 Vue + CSS3
*/

/* 首先给所有元素加上过渡效果 */

* {
  -webkit-transition: all .3s;
  transition: all .3s;
}

/* 白色背景太单调了，我们来点背景 */

html {
  color: rgb(222,222,222); background: rgb(0,43,54);
}

/* 文字离边框太近了 */

.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  width: 45vw; height: 90vh;
}

/* 代码高亮 */

.token.selector{ color: rgb(133,153,0); }
.token.property{ color: rgb(187,137,0); }
.token.punctuation{ color: yellow; }
.token.function{ color: rgb(42,161,152); }

/* 加点 3D 效果呗 */

html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.styleEditor {
  position: fixed; left: 0; top: 0;
  -webkit-transition: none;
  transition: none;
  -webkit-transform: rotateY(10deg) translateZ(-100px) ;
          transform: rotateY(10deg) translateZ(-100px) ;
}

/* 接下来我给自己准备一个编辑器 */

.resumeEditor{
  position: fixed; right: 0; top: 0;
  padding: .5em;  margin: .5em;
  width: 48vw; height: 90vh;
  border: 1px solid;
  background: white; color: #222;
  overflow: auto;
}

/* 好了，我开始写简历了 */


/* 这个简历好像差点什么
 * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
 * 简单，用开源工具翻译成 HTML 就行了
 */
                `
            }
        },
        created(){
            let n = 0;
            let timer = setInterval(() => {
                if (n === this.finalCode.length) {
                    clearInterval(timer);
                    console.log(`end`)
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

