<template>
    <div id="app">
        <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
        <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
    </div>
</template>

<script>
    import StyleEditor from './components/StyleEditor.vue'
    import ResumeEditor from './components/ResumeEditor.vue'
    import './assets/reset.css'

    export default {
        name: 'app',
        components: {
            StyleEditor,
            ResumeEditor
        },
        data () {
            return {
                interval: 50,
                currentStyle: ``,
                fullStyle: [`
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

`, `
/* 这个简历好像差点什么
 * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
 * 简单，用开源工具翻译成 HTML 就行了
 */
                `],
                enableHtml: false,
                currentMarkdown: ``,
                fullMarkdown: `李子翔
----

个人网站 http://www.lizixiang.cn

技能
----

* 技术栈
* Vue.js 开发
* React.js 开发
* Node.js 开发
* ES6

工作经历
----

1. 携程
2. 上海忆久软件有限公司

链接
----

* [GitHub](https://github.com/myvipbackup2)
* [我的博客](http://blog.lizixiang.cn)

> 如果你喜欢这个效果，Fork [我的项目](https://github.com/myvipbackup2/vue-resume)，打造你自己的简历！

`
            }
        },
        created(){
            this.makeResume();
        },
        methods: {
            makeResume: async function () {
                await this.progressivelyShowStyle(0); //数组第一个,数组方便折叠字符串模板
                await this.progressivelyShowResume();
                await this.progressivelyShowStyle(1);
                await this.showHtml();
                await this.progressivelyShowStyle(2);
            },
            showHtml(){
                return new Promise((resolve, reject) => {
                    this.enableHtml = true;
                    resolve();
                })
            },
            progressivelyShowStyle(n) {
                return new Promise((resolve, reject) => {
                    let interval = this.interval;
                    const showStyle = (async function () {
                        let style = this.fullStyle[n];
                        if (!style) {
                            return
                        }
                        // 计算前 n 个 style 的字符总数
                        let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0);
                        let prefixLength = length - style.length;
                        if (this.currentStyle.length < length) {
                            let l = this.currentStyle.length - prefixLength;
                            let char = style.substring(l, l + 1) || ' ';
                            this.currentStyle += char;
                            // 如果是一个换行，滚动条则向下滚动到底，形成打代码可以自动滚动
                            if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                                this.$nextTick(() => {
                                    this.$refs.styleEditor.goBottom();
                                })
                            }
                            setTimeout(showStyle, interval)
                        } else {
                            resolve()
                        }
                    }).bind(this);
                    showStyle();
                })
            },
            progressivelyShowResume() {
                return new Promise((resolve, reject) => {
                    let length = this.fullMarkdown.length;
                    let interval = this.interval;
                    let showResume = () => {
                        if (this.currentMarkdown.length < length) {
                            this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1);
                            let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1];
                            let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2];
                            console.log(prevChar);
                            if (prevChar === '\n' && this.$refs.resumeEditor) {
                                this.$nextTick(() => this.$refs.resumeEditor.goBottom())
                            }
                            setTimeout(showResume, interval)
                        } else {
                            resolve()
                        }
                    };
                    showResume()
                })
            }
        }
    }
</script>


<style scoped>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }

    html {
        min-height: 100vh;
    }

    * {
        -webkit-transition: all .3s;
        transition: all .3s;
    }
</style>

