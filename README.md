# vue-survey

> Vue根据后台数据动态生成的调查问卷页面（适配移动端），其中textarea可以自适应改变高度；

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 正式内容：

### 一、单选

为了更详细的描述，将先写死一个数据，再到后面抽离成组件更方便使用和数据的交互。


##### 1. 编写基本页面

![](https://upload-images.jianshu.io/upload_images/7016617-d1dfdda51eb0ad87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


HelloWorld.vue

```
<template>
  <div class="hello">
    <div class="question">
      <div class="num">01</div>
      <div class="content">
        <div class="title">单选题：今年是哪一年</div>
        <div class="my-radio">
          <div class="row">
            <div class="radio radio-normal"></div>
            <div class="label">2017年</div>
          </div>
          <div class="row">
            <div class="radio radio-normal"></div>
            <div class="label">2018年</div>
          </div>
          <div class="row">
            <div class="radio radio-active"></div>
            <div class="label">2019年</div>
          </div>
          <div class="row">
            <div class="radio radio-normal"></div>
            <div class="label">2020年</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      return {
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .question {
    margin-bottom: 38px;
    position: relative;
    display: inline-block;
    width: 680px;
    height: auto;
    background-color: #fffcf0;
    border-radius: 20px;
    z-index: 0;
    .num {
      position: absolute;
      left: 20px;
      top: 0;
      width: 54px;
      height: 66px;
      line-height: 66px;
      font-size: 30px;
      color: #fff;
      background-color: #FCB882;
    }
    .content {
      margin-left: 20px;
      margin-top: 38px;
      display: inline-block;
      width: 520px;
      .title {
        margin-bottom: 38px;
        line-height: 1.5em;
        color: #976e48;
        font-weight: bold;
        font-size: 30px;
        text-align: left;
      }
      /*background: #ff1d5e;*/
    }
    .my-radio {
      text-align: left;
      .row {
        margin-bottom: 38px;
        position: relative;
        float: left;
        display: inline-block;
        width: 100%;
        line-height: 38px;
        color: #976e48;
        font-size: 30px;
        cursor: pointer;
        -webkit-tap-highlight-color:rgba(0,0,0,0);
        .radio {
          margin-top: 7px;
          vertical-align: top;
          display: inline-block;
          width: 24px;
          height: 24px;
          background-size: 100%;
          background-repeat: no-repeat;
          &-active {
            background-image: url("../assets/images/radio-active.png");
          }
          &-normal {
            background-image: url("../assets/images/radio-normal.png");
          }
        }
        .label, .input-container {
          display: inline-block;
          /*margin-left: 5px;*/
          width: calc(100% - 24px - 15px);
        }
        .input-container {
          .text {
            float: left;
            /*background-color: #ff1d5e;*/
          }
          .input {
            padding: 0 0;
            margin-left: 10px;
            float: left;
            width: calc(100% - 4em);
          }
        }
      }
    }
  }
</style>
```


##### 2. 抽象单选选项数据radioData

`label`为显示的选项名，`value`为选中这个选项的值，用`status`来判断是否被选中：


```
<template>
  <div class="hello">
    <div class="question">
      <div class="num">01</div>
      <div class="content">
        <div class="title">单选题：今年是哪一年</div>
        <div class="my-radio">
          <div v-for="item in radioData" class="row">
            <div :class="['radio', item.status ? 'radio-active' :'radio-normal']"></div>
            <div class="label">{{ item.label }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      return {
        radioData: [
          {
            label: '2017年',
            value: 'a',
            status: false
          },
          {
            label: '2018年',
            value: 'b',
            status: false
          },
          {
            label: '2019年',
            value: 'c',
            status: true
          },
          {
            label: '2020年',
            value: 'd',
            status: false
          },
        ]
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .question {
    margin-bottom: 38px;
    position: relative;
    display: inline-block;
    width: 680px;
    height: auto;
    background-color: #fffcf0;
    border-radius: 20px;
    z-index: 0;
    .num {
      position: absolute;
      left: 20px;
      top: 0;
      width: 54px;
      height: 66px;
      line-height: 66px;
      font-size: 30px;
      color: #fff;
      background-color: #FCB882;
    }
    .content {
      margin-left: 20px;
      margin-top: 38px;
      display: inline-block;
      width: 520px;
      .title {
        margin-bottom: 38px;
        line-height: 1.5em;
        color: #976e48;
        font-weight: bold;
        font-size: 30px;
        text-align: left;
      }
      /*background: #ff1d5e;*/
    }
    .my-radio {
      text-align: left;
      .row {
        margin-bottom: 38px;
        position: relative;
        float: left;
        display: inline-block;
        width: 100%;
        line-height: 38px;
        color: #976e48;
        font-size: 30px;
        cursor: pointer;
        -webkit-tap-highlight-color:rgba(0,0,0,0);
        .radio {
          margin-top: 7px;
          vertical-align: top;
          display: inline-block;
          width: 24px;
          height: 24px;
          background-size: 100%;
          background-repeat: no-repeat;
          &-active {
            background-image: url("../assets/images/radio-active.png");
          }
          &-normal {
            background-image: url("../assets/images/radio-normal.png");
          }
        }
        .label, .input-container {
          display: inline-block;
          /*margin-left: 5px;*/
          width: calc(100% - 24px - 15px);
        }
        .input-container {
          .text {
            float: left;
            /*background-color: #ff1d5e;*/
          }
          .input {
            padding: 0 0;
            margin-left: 10px;
            float: left;
            width: calc(100% - 4em);
          }
        }
      }
    }
  }
</style>
```


##### 3. 加上点击事件，满足单选功能


 ```
<template>
  <div class="hello">
    <div class="question">
      <div class="num">01</div>
      <div class="content">
        <div class="title">单选题：今年是哪一年</div>
        <div class="my-radio">
          <div v-for="item in radioData" class="row" @click="change(item)">
            <div :class="['radio', item.status ? 'radio-active' :'radio-normal']"></div>
            <div class="label">{{ item.label }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      return {
        radioData: [
          {
            label: '2017年',
            value: 'a',
            status: false
          },
          {
            label: '2018年',
            value: 'b',
            status: false
          },
          {
            label: '2019年',
            value: 'c',
            status: true
          },
          {
            label: '2020年',
            value: 'd',
            status: false
          },
        ]
      }
    },
    methods: {
      change(data) {
        for (let item of this.radioData) {
          // 如果值相等，即为点击的数据，状态为active，其他normal
          item.status = data.value === item.value;
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .question {
    margin-bottom: 38px;
    position: relative;
    display: inline-block;
    width: 680px;
    height: auto;
    background-color: #fffcf0;
    border-radius: 20px;
    z-index: 0;
    .num {
      position: absolute;
      left: 20px;
      top: 0;
      width: 54px;
      height: 66px;
      line-height: 66px;
      font-size: 30px;
      color: #fff;
      background-color: #FCB882;
    }
    .content {
      margin-left: 20px;
      margin-top: 38px;
      display: inline-block;
      width: 520px;
      .title {
        margin-bottom: 38px;
        line-height: 1.5em;
        color: #976e48;
        font-weight: bold;
        font-size: 30px;
        text-align: left;
      }
      /*background: #ff1d5e;*/
    }
    .my-radio {
      text-align: left;
      .row {
        margin-bottom: 38px;
        position: relative;
        float: left;
        display: inline-block;
        width: 100%;
        line-height: 38px;
        color: #976e48;
        font-size: 30px;
        cursor: pointer;
        -webkit-tap-highlight-color:rgba(0,0,0,0);
        .radio {
          margin-top: 7px;
          vertical-align: top;
          display: inline-block;
          width: 24px;
          height: 24px;
          background-size: 100%;
          background-repeat: no-repeat;
          &-active {
            background-image: url("../assets/images/radio-active.png");
          }
          &-normal {
            background-image: url("../assets/images/radio-normal.png");
          }
        }
        .label, .input-container {
          display: inline-block;
          /*margin-left: 5px;*/
          width: calc(100% - 24px - 15px);
        }
        .input-container {
          .text {
            float: left;
            /*background-color: #ff1d5e;*/
          }
          .input {
            padding: 0 0;
            margin-left: 10px;
            float: left;
            width: calc(100% - 4em);
          }
        }
      }
    }
  }
</style>
```


##### 4.抽象整个组件


然后再把一整个单选组件独立出来，在子组件里管理数据变化，父组件传初始值进去，变化后也只需要知道结果就行了。


这里我们先假设有两道`radio`单选题：



4.1. 首先，新增一个组件`MyRadio.vue`。


![](https://upload-images.jianshu.io/upload_images/7016617-ed1d277c25df3f95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



+ 将写在`HelloWorld.vue`里的关于单选选项的`HTML`、`CSS`、以及点击选项改变状态的`change`方法都独立在`MyRadio.vue`里：
```
<template>
  <div class="my-radio">
    <div v-for="item in radioData" class="row" @click="change(item)">
      <div :class="['radio', item.status ? 'radio-active' :'radio-normal']"></div>
      <div class="label">{{ item.label }}</div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'my-radio',
    data() {
      return {
      }
    },
    methods: {
      change(data) {
        for (let item of this.radioData) {
          // 如果值相等，即为点击的数据，状态为active，其他normal
          item.status = data.value === item.value;
        }
      },
    }
  }
</script>

<style scoped lang="scss">
  .my-radio {
    text-align: left;
    .row {
      margin-bottom: 38px;
      position: relative;
      float: left;
      display: inline-block;
      width: 100%;
      line-height: 38px;
      color: #976e48;
      font-size: 30px;
      cursor: pointer;
      -webkit-tap-highlight-color:rgba(0,0,0,0);
      .radio {
        margin-top: 7px;
        vertical-align: top;
        display: inline-block;
        width: 24px;
        height: 24px;
        background-size: 100%;
        background-repeat: no-repeat;
        &-active {
          background-image: url("../assets/images/radio-active.png");
        }
        &-normal {
          background-image: url("../assets/images/radio-normal.png");
        }
      }
      .label, .input-container {
        display: inline-block;
        /*margin-left: 5px;*/
        width: calc(100% - 24px - 15px);
      }
      .input-container {
        .text {
          float: left;
          /*background-color: #ff1d5e;*/
        }
        .input {
          padding: 0 0;
          margin-left: 10px;
          float: left;
          width: calc(100% - 4em);
        }
      }
    }
  }
</style>
```


+ 由于`radioData`是应该从父组件传递过来的，所以加在`props`里，为避免以后调用组件会忘记，加上`required: true`：


```
props: {
  radioData: {
  required: true
  }
},
```


+ 虽然子组件是可以直接修改父组件传递过来的数据类型对象或者数组的数据，但为了更优雅更独立，不应在子组件里执行修改数据，所以`change`函数要修改，改为`$emit`父组件`radioData`该变化了，且把要修改的值传过去：
```
change(data) {
  this.$emit('radioChange', data);
},
```


+ 由于这里我们假设有两道单选题，但改变了其中某一道选项时，另外一道应该不会被干扰，所以在`change`方法还应该再传回个标识，告诉父组件是其中那道题的修改了，这个标识用`radioName`表示，应该从父组件传递过来，则`props`应该再加一个：
```
props: {
  radioData: {
    required: true
  },
  // 新加的
  radioName: {
    required: true
  }
},
```


`change`方法：


```
methods: {
  change(data) {
    this.$emit('radioChange', data, this.radioName);
  },
}
```


4.2. 在`HelloWorld`组件中调用`MyRadio`组件


+ 在引入之前，我们先把两道单选题的数据封装一下，命名为`questionData`，数据格式为数组；每一个对象对应每一道题目作为问卷数组的一个元素，对象里应有个id（这道题的id），title（题目），type（类型，这里单选为radio，之后还会有多选checkbox等等），data（选项数据）。


```
data () {
    return {
      questionData: [
        {
          id: 1,
          title: '单选题：今年是哪一年？',
          type: 'radio',
          data: [
            {
              label: '2017年',
              value: 'a',
              status: false
            },
            {
              label: '2018年',
              value: 'b',
              status: false
            },
            {
              label: '2019年',
              value: 'c',
              status: false
            },
            {
              label: '2020年',
              value: 'd',
              status: false
            },
          ]
        },
        {
          id: 2,
          title: '单选题：广东省的冬天会下雪吗？',
          type: 'radio',
          data: [
            {
              label: '会',
              value: 'a',
              status: false
            },
            {
              label: '不会',
              value: 'b',
              status: false
            }
          ]
        },
      ]
    }
  },
```


+ 引用`MyRadio`组件

```
  import MyRadio from './MyRadio'
```


```
components: {
      MyRadio
    },
```


+ 循环渲染出`questionData`里的元素，加一个判断，如果`type`为`radio`才调用`MyRadio`组件，并传入必填的radioData和radioName属性，radioName属性把当前这题目的index传过去作为标识，方便后续修改（`this.questionData[index]`）就能获取到当前题目的数据；
```
<template>
  <div class="hello">
    <div v-for="(item, index) in questionData" class="question">
      <div class="num">{{ index + 1 }}</div>
      <div class="content">
        <div class="title">{{ item.title }}</div>
        <template v-if="item.type === 'radio'">
          <my-radio :radioData="item.data" :radioName="index"></my-radio>
        </template>
      </div>
    </div>
  </div>
</template>
```


![](https://upload-images.jianshu.io/upload_images/7016617-aef3cbce3ea470a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




可以看到界面在这个时候已经完成了，但是由于我们没做当子组件里change函数触发了父组件的radioChange方法的调用，所以怎么点选项都没有反应，补上：


```
methods: {
      radioChange(data, name) {
        const question = this.questionData[name];
        for (let item of question.data) {
          item.status = data.value === item.value;
        }
      }
    }
```



![两道题互不干扰呢](https://upload-images.jianshu.io/upload_images/7016617-e84e2b947b91f78e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 二、多选


多选和单选其实差不多，就是当点击后数据的处理不一样，我们参照着单选的`MyRadio`组件，写一个`MyCheckbox`组件：


```
<template>
    <div class="my-checkbox">
      <div v-for="item in checkboxData" class="row" @click="change(item)">
        <div :class="['checkbox', item.status ? 'checkbox-active' :'checkbox-normal']"></div>
        <div class="label">{{ item.label }}</div>
      </div>
    </div>
</template>

<script>
  export default {
    name: 'my-checkbox',
    props: {
      checkboxData: {
        required: true
      },
      // 新加的
      checkboxName: {
        required: true
      }
    },
    data() {
      return {
      }
    },
    methods: {
      change(data) {
        this.$emit('checkboxChange', data, this.checkboxName);
      },
    }
  }
</script>

<style scoped lang="scss">
  .my-checkbox {
    text-align: left;
    .row {
      margin-bottom: 38px;
      position: relative;
      float: left;
      display: inline-block;
      width: 100%;
      line-height: 38px;
      color: #976e48;
      font-size: 30px;
      cursor: pointer;
      -webkit-tap-highlight-color: rgba(0,0,0,0);
      .checkbox {
        margin-top: 0;
        vertical-align: top;
        display: inline-block;
        width: 30px;
        height: 32px;
        background-size: 100%;
        background-repeat: no-repeat;
        &-active {
          background-image: url("../assets/images/checkbox-active.png");
        }
        &-normal {
          background-image: url("../assets/images/checkbox-normal.png");
        }
      }
      .label, .input-container {
        display: inline-block;
        /*margin-left: 5px;*/
        width: calc(100% - 24px - 15px);
      }
      .input-container {
        .text {
          float: left;
          /*background-color: #ff1d5e;*/
        }
        .input {
          padding: 0 0;
          margin-left: 10px;
          float: left;
          width: calc(100% - 4em);
        }
      }
    }
  }
</style>
```


+ 在HelloWorld.vue中引用：



```
<template>
  <div class="hello">
    <div v-for="(item, index) in questionData" class="question">
      <div class="num">{{ index + 1 }}</div>
      <div class="content">
        <div class="title">{{ item.title }}</div>
        <template v-if="item.type === 'radio'">
          <my-radio :radioData="item.data" :radioName="index" @radioChange="radioChange"></my-radio>
        </template>
        <template v-else-if="item.type === 'checkbox'">
          <my-checkbox :checkboxData="item.data" :checkboxName="index" @checkboxChange="checkboxChange"></my-checkbox>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
  import MyRadio from './MyRadio';
  import MyCheckbox from './MyCheckbox'
  export default {
    name: 'HelloWorld',
    components: {
      MyRadio, MyCheckbox
    },
    data () {
      return {
        questionData: [
          {
            id: 1,
            title: '单选题：今年是哪一年？',
            type: 'radio',
            data: [
              {
                label: '2017年',
                value: 'a',
                status: false
              },
              {
                label: '2018年',
                value: 'b',
                status: false
              },
              {
                label: '2019年',
                value: 'c',
                status: false
              },
              {
                label: '2020年',
                value: 'd',
                status: false
              },
            ]
          },
          {
            id: 2,
            title: '单选题：广东省的冬天会下雪吗？',
            type: 'radio',
            data: [
              {
                label: '会',
                value: 'a',
                status: false
              },
              {
                label: '不会',
                value: 'b',
                status: false
              }
            ]
          },
          {
            id: 3,
            title: '多选题：你喜欢的季节有？',
            type: 'checkbox',
            data: [
              {
                label: '春天',
                value: 'a',
                status: false
              },
              {
                label: '夏天',
                value: 'b',
                status: false
              },
              {
                label: '秋天',
                value: 'c',
                status: false
              },
              {
                label: '冬天',
                value: 'd',
                status: false
              }
            ]
          },
        ]
      }
    },
    methods: {
      radioChange(data, name) {
        const question = this.questionData[name];
        for (let item of question.data) {
          item.status = data.value === item.value;
        }
      },
      checkboxChange(data, name) {
        console.log(data,name)
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .question {
    margin-bottom: 38px;
    position: relative;
    display: inline-block;
    width: 680px;
    height: auto;
    background-color: #fffcf0;
    border-radius: 20px;
    z-index: 0;
    .num {
      position: absolute;
      left: 20px;
      top: 0;
      width: 54px;
      height: 66px;
      line-height: 66px;
      font-size: 30px;
      color: #fff;
      background-color: #FCB882;
    }
    .content {
      margin-left: 20px;
      margin-top: 38px;
      display: inline-block;
      width: 520px;
      .title {
        margin-bottom: 38px;
        line-height: 1.5em;
        color: #976e48;
        font-weight: bold;
        font-size: 30px;
        text-align: left;
      }
      /*background: #ff1d5e;*/
    }
  }
</style>
```


![页面如图所示](https://upload-images.jianshu.io/upload_images/7016617-37dc0ba259ae307d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



+ 完善`checkboxChange`事件，当点击的时候，应该把当前选项改为相反状态。


```
checkboxChange(data, name) {
        const question = this.questionData[name];
        const index = question.data.findIndex(item => item.value === data.value);
        question.data[index].status = !question.data[index].status;
}
```


![可以多选啦](https://upload-images.jianshu.io/upload_images/7016617-4e61928dc782202f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 三、填空


填空的由于要实现自动换行，所以采用的是`textarea`而不是`input`，具体怎么实现自适应高度的，可参考我的另外一篇简书文章：[Vue中可根据内容自适应改变高度的textarea文本框]()。这里我们在那篇简书的基础上，新增以下：

+ 好看的样式

+ 最多字数限制

+ placeholder内容

+ 内容改变触发父组件的`changeTextarea`方法





MyTextarea.vue


```
<template>
  <div class="my-textarea">
    <textarea ref="textarea" :style="{'height': height}" :maxlength="maxLength" v-model="value" :placeholder="placeholder" class="textarea" ></textarea>
  </div>
</template>

<script>
  import calcTextareaHeight from '../assets/calcTextareaHeight';
  export default {
    name: 'aw-textarea',
    props: {
      // 用来辨别是哪个数据调用的
      textareaName: {
        required: true
      },
      // placeholder显示的内容
      placeholder: {
        default: '请填写'
      },
      // 最多字数
      maxLength: {
        default: 200
      }
    },
    data() {
      return {
        // textarea内容
        value: '',
        // 动态高度
        height: 0
      }
    },
    watch: {
      value() {
        if (this.$refs.textarea.value.length >= this.maxLength) {
          this.$toasted.show(`最多可以填写${this.maxLength}字哦~`, {
            position: 'top-center',
            duration: 3000
          })
        }
        this.getHeight();
        this.$emit('changeTextarea', this.value, this.textareaName);
      }
    },
    mounted() {
      this.getHeight();
    },
    methods: {
      getHeight() {
        this.height = calcTextareaHeight(this.$refs.textarea, 1, null).height;
      }
    }
  }
</script>

<style scoped lang="scss">
  .my-textarea {
    .textarea {
      padding: 0;
      position: relative;
      top: -2px;
      display: inline-block;
      width: 100%;
      height: 100px;
      line-height: 38px;
      /*height: auto;*/
      font-size: 30px;
      resize: none;
      color: #976e48;
      border-radius: 0;
      border: none;
      border-bottom: 1px solid #f8bc77;
      background-color: #fffcf0;
    }
  }
</style>
```



HelloWorld.vue



```
<template>
  <div class="hello">
    <div v-for="(item, index) in questionData" class="question">
      <div class="num">{{ index + 1 }}</div>
      <div class="content">
        <div class="title">{{ item.title }}</div>
        <template v-if="item.type === 'radio'">
          <my-radio :radioData="item.data" :radioName="index" @radioChange="radioChange"></my-radio>
        </template>
        <template v-else-if="item.type === 'checkbox'">
          <my-checkbox :checkboxData="item.data" :checkboxName="index" @checkboxChange="checkboxChange"></my-checkbox>
        </template>
        <template v-else-if="item.type === 'textarea'">
          <my-textarea :textareaName="index" style="margin-bottom: 38px;" @changeTextarea="changeTextarea"></my-textarea>
        </template>
      </div>
    </div>

  </div>
</template>

<script>
  import MyRadio from './MyRadio';
  import MyCheckbox from './MyCheckbox';
  import MyTextarea from './MyTextarea';
  export default {
    name: 'HelloWorld',
    components: {
      MyRadio, MyCheckbox, MyTextarea
    },
    data () {
      return {
        questionData: [
          {
            id: 1,
            title: '单选题：今年是哪一年？',
            type: 'radio',
            data: [
              {
                label: '2017年',
                value: 'a',
                status: false
              },
              {
                label: '2018年',
                value: 'b',
                status: false
              },
              {
                label: '2019年',
                value: 'c',
                status: false
              },
              {
                label: '2020年',
                value: 'd',
                status: false
              },
            ]
          },
          {
            id: 2,
            title: '单选题：广东省的冬天会下雪吗？',
            type: 'radio',
            data: [
              {
                label: '会',
                value: 'a',
                status: false
              },
              {
                label: '不会',
                value: 'b',
                status: false
              }
            ]
          },
          {
            id: 3,
            title: '多选题：你喜欢的季节有？',
            type: 'checkbox',
            data: [
              {
                label: '春天',
                value: 'a',
                status: false
              },
              {
                label: '夏天',
                value: 'b',
                status: false
              },
              {
                label: '秋天',
                value: 'c',
                status: false
              },
              {
                label: '冬天',
                value: 'd',
                status: false
              }
            ]
          },
          {
            id: 4,
            title: '填空题：请问还有什么疑问吗？',
            type: 'textarea',
            data: ''
          },
        ]
      }
    },
    methods: {
      radioChange(data, name) {
        const question = this.questionData[name];
        for (let item of question.data) {
          item.status = data.value === item.value;
        }
      },
      checkboxChange(data, name) {
        const question = this.questionData[name];
        const index = question.data.findIndex(item => item.value === data.value);
        question.data[index].status = !question.data[index].status;
      },
      changeTextarea(data, name) {
        const question = this.questionData[name];
        question.data = data;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .question {
    margin-bottom: 38px;
    position: relative;
    display: inline-block;
    width: 680px;
    height: auto;
    background-color: #fffcf0;
    border-radius: 20px;
    z-index: 0;
    .num {
      position: absolute;
      left: 20px;
      top: 0;
      width: 54px;
      height: 66px;
      line-height: 66px;
      font-size: 30px;
      color: #fff;
      background-color: #FCB882;
    }
    .content {
      margin-left: 20px;
      margin-top: 38px;
      display: inline-block;
      width: 520px;
      .title {
        margin-bottom: 38px;
        line-height: 1.5em;
        color: #976e48;
        font-weight: bold;
        font-size: 30px;
        text-align: left;
      }
      /*background: #ff1d5e;*/
    }
  }
</style>
```


![](https://upload-images.jianshu.io/upload_images/7016617-815a32978154df29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 四、在单选中引入填空


其实就是在单选中调用`MyTextarea`组件，然后再处理一下当选了要填空的选项、填了内容后传值给父组件的情况。

1. 在`HelloWorld.vue`中的`questionData`数据中的单选数据的每个选项数据里都要加个type，判断是普通单选(1)，还是要填空(2)。


```
questionData: [
    {
      id: 1,
      title: '单选题：今年是哪一年？',
      type: 'radio',
      data: [
        {
          type: 1,
          label: '2017年',
          value: 'a',
          status: false
        },
        {
          type: 1,
          label: '2018年',
          value: 'b',
          status: false
        },
        {
          type: 1,
          label: '2019年',
          value: 'c',
          status: false
        },
        {
          type: 1,
          label: '2020年',
          value: 'd',
          status: false
        },
        {
          type: 2,
          label: '其他',
          value: '',
          status: false
        }
      ]
    },
    {
      id: 2,
      title: '单选题：广东省的冬天会下雪吗？',
      type: 'radio',
      data: [
        {
          type: 1,
          label: '会',
          value: 'a',
          status: false
        },
        {
          type: 1,
          label: '不会',
          value: 'b',
          status: false
        }
      ]
    },
    {
      id: 3,
      title: '多选题：你喜欢的季节有？',
      type: 'checkbox',
      data: [
        {
          type: 1,
          label: '春天',
          value: 'a',
          status: false
        },
        {
          type: 1,
          label: '夏天',
          value: 'b',
          status: false
        },
        {
          type: 1,
          label: '秋天',
          value: 'c',
          status: false
        },
        {
          type: 1,
          label: '冬天',
          value: 'd',
          status: false
        }
      ]
    },
    {
      id: 4,
      title: '填空题：请问还有什么疑问吗？',
      type: 'textarea',
      data: ''
    },
  ]
```

2.在`MyRadio`中加入判断，以及为填空的样式(原来之前已经加了，只是没用)，当填空内容发生改变时候被子组件`MyTextarea`触发的`changeTextarea`方法，整个代码`MyRadio.vue`为：
```
<template>
  <div class="my-radio">
    <div v-for="(item, index) in radioData" @click="change(item)" class="row">
      <div :class="['radio', item.status ? 'radio-active' :'radio-normal']"></div>
      <div v-if="item.type === 2" class="input-container">
        <div class="text">其他</div>
        <div class="input">
          <my-textarea :textareaName="index" :placeholder="'请输入年份'" :maxLength="100" @changeTextarea="changeTextarea"></my-textarea>
        </div>
      </div>
      <div v-else class="label">{{ item.label }}</div>
    </div>
  </div>
</template>

<script>
  import MyTextarea from './MyTextarea'
  export default {
    name: 'my-radio',
    components: {
      MyTextarea
    },
    props: {
      radioData: {
        required: true
      },
      // 新加的
      radioName: {
        required: true
      }
    },
    data() {
      return {
      }
    },
    methods: {
      change(data) {
        this.$emit('radioChange', data, this.radioName);
      },
      changeTextarea(value, name) {
        const data = this.radioData.concat([])[name];
        data.value = value;
        this.$emit('radioChange', data, this.radioName);
      }
    }
  }
</script>

<style scoped lang="scss">
  .my-radio {
    text-align: left;
    .row {
      margin-bottom: 38px;
      position: relative;
      float: left;
      display: inline-block;
      width: 100%;
      line-height: 38px;
      color: #976e48;
      font-size: 30px;
      cursor: pointer;
      -webkit-tap-highlight-color:rgba(0,0,0,0);
      .radio {
        margin-top: 7px;
        vertical-align: top;
        display: inline-block;
        width: 24px;
        height: 24px;
        background-size: 100%;
        background-repeat: no-repeat;
        &-active {
          background-image: url("../assets/images/radio-active.png");
        }
        &-normal {
          background-image: url("../assets/images/radio-normal.png");
        }
      }
      .label, .input-container {
        display: inline-block;
        /*margin-left: 5px;*/
        width: calc(100% - 24px - 15px);
      }
      .input-container {
        .text {
          float: left;
          /*background-color: #ff1d5e;*/
        }
        .input {
          padding: 0 0;
          margin-left: 10px;
          float: left;
          width: calc(100% - 4em);
        }
      }
    }
  }
</style>
```



![image.png](https://upload-images.jianshu.io/upload_images/7016617-cca69631b2de0611.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




### 五、在多选中引入填空


基本同理单选中引入填空，就不详讲了。



MyCheckBox.vue
```
<template>
    <div class="my-checkbox">
      <div v-for="(item, index) in checkboxData" class="row" @click="change(item)">
        <div :class="['checkbox', item.status ? 'checkbox-active' :'checkbox-normal']"></div>
        <div v-if="item.type === 2" class="input-container">
          <div class="text">其他</div>
          <div class="input">
            <my-textarea :textareaName="index" :maxLength="100" @changeTextarea="changeTextarea"></my-textarea>
          </div>
        </div>
        <div v-else class="label">{{ item.label }}</div>
      </div>
    </div>
</template>

<script>
  import MyTextarea from './MyTextarea';
  export default {
    name: 'my-checkbox',
    components: {
      MyTextarea
    },
    props: {
      checkboxData: {
        required: true
      },
      // 新加的
      checkboxName: {
        required: true
      }
    },
    data() {
      return {
      }
    },
    methods: {
      change(data) {
        this.$emit('checkboxChange', data, this.checkboxName);
      },
      changeTextarea(value, name) {
        const data = this.checkboxData.concat([])[name];
        data.value = value;
        this.$emit('checkboxChange', data, this.checkboxName, true);
      }
    }
  }
</script>

<style scoped lang="scss">
  .my-checkbox {
    text-align: left;
    .row {
      margin-bottom: 38px;
      position: relative;
      float: left;
      display: inline-block;
      width: 100%;
      line-height: 38px;
      color: #976e48;
      font-size: 30px;
      cursor: pointer;
      -webkit-tap-highlight-color: rgba(0,0,0,0);
      .checkbox {
        margin-top: 0;
        vertical-align: top;
        display: inline-block;
        width: 30px;
        height: 32px;
        background-size: 100%;
        background-repeat: no-repeat;
        &-active {
          background-image: url("../assets/images/checkbox-active.png");
        }
        &-normal {
          background-image: url("../assets/images/checkbox-normal.png");
        }
      }
      .label, .input-container {
        display: inline-block;
        /*margin-left: 5px;*/
        width: calc(100% - 24px - 15px);
      }
      .input-container {
        .text {
          float: left;
          /*background-color: #ff1d5e;*/
        }
        .input {
          padding: 0 0;
          margin-left: 10px;
          float: left;
          width: calc(100% - 4em);
        }
      }
    }
  }
</style>
```



HelloWorld.vue：



```
<template>
  <div class="my-textarea">
    <textarea ref="textarea" :style="{'height': height}" :maxlength="maxLength" v-model="value" :placeholder="placeholder" class="textarea" ></textarea>
  </div>
</template>

<script>
  import calcTextareaHeight from '../assets/calcTextareaHeight';
  export default {
    name: 'aw-textarea',
    props: {
      // 用来辨别是哪个数据调用的
      textareaName: {
        required: true
      },
      // placeholder显示的内容
      placeholder: {
        default: '请填写'
      },
      // 最多字数
      maxLength: {
        default: 200
      }
    },
    data() {
      return {
        // textarea内容
        value: '',
        // 动态高度
        height: 0
      }
    },
    watch: {
      value() {
        if (this.$refs.textarea.value.length >= this.maxLength) {
          this.$toasted.show(`最多可以填写${this.maxLength}字哦~`, {
            position: 'top-center',
            duration: 3000
          })
        }
        this.getHeight();
        this.$emit('changeTextarea', this.value, this.textareaName);
      }
    },
    mounted() {
      this.getHeight();
    },
    methods: {
      getHeight() {
        this.height = calcTextareaHeight(this.$refs.textarea, 1, null).height;
      }
    }
  }
</script>

<style scoped lang="scss">
  .my-textarea {
    .textarea {
      padding: 0;
      position: relative;
      top: -2px;
      display: inline-block;
      width: 100%;
      height: 100px;
      line-height: 38px;
      /*height: auto;*/
      font-size: 30px;
      resize: none;
      color: #976e48;
      border-radius: 0;
      border: none;
      border-bottom: 1px solid #f8bc77;
      background-color: #fffcf0;
    }
  }
</style>
```


![](https://upload-images.jianshu.io/upload_images/7016617-15dd9fa0202fee90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 六、获取且组装整个问卷的答案（表单数据）


这里就从简了，没有加是否为空之类的判断。以下的改动均在`HelloWorld.vue`页面里：

1.新增个按钮，给按钮加上点击事件`handleSubmit`。


```
<button @click="handleSubmit" class="btn">点击提交</button>
```


2.这里的表单有三种类型：单选、多选、填空。与后台约定好的理想的组装的格式应该为（其他格式都能组装，重点是拿到数据和判断）：


```
const params = {
  "第一道题目(单选题)的id": '选中的选项的value值，或者是选了填写的输入值',
  "第二道题目(单选题)的id": '选中的选项的value值，或者是选了填写的输入值',
  "第三道题目(多选题)的id": ['选中的选项的value值', '选中的选项的value值'],
  "第四道题目(填空题)的id": '填写的输入值',
  "第五道题目(多选题)的id": ['选中的选项的value值', '选中的选项的value值']
}
```



3.组装数据

```
      handleSubmit() {
        const data = this.questionData;
        const params = {};
        for (let question of data) {
          if (question.type === 'radio') {
            const index = question.data.findIndex(item => item.status);
            if (index !== -1) {
              params[question.id] = question.data[index].value;
            } else {
              params[question.id] = '';
            }
          } else if (question.type === 'checkbox') {
            const arr = [];
            for (let item of question.data) {
              if (item.status) {
                arr.push(item.value);
              }
            }
            params[question.id] = arr;
          } else if (question.type === 'textarea') {
            params[question.id] = question.data;
          }
        }
        // console.log(data[0]);
        console.log(params);
      }
    }
```

1. 啥都没填，直接按提交按钮的时候


![](https://upload-images.jianshu.io/upload_images/7016617-95e79294a77ebee8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



2.很认真的如图所示填了

![](https://upload-images.jianshu.io/upload_images/7016617-85e0cc7360fa27c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/7016617-3afc367c06a106a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![](https://upload-images.jianshu.io/upload_images/7016617-c7871151697e1467.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



