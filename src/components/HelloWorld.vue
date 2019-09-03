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
    <button @click="handleSubmit" class="btn">点击提交</button>
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
          {
            id: 5,
            title: '多选题：你喜欢的水果有？',
            type: 'checkbox',
            data: [
              {
                type: 1,
                label: '苹果',
                value: 'a',
                status: false
              },
              {
                type: 1,
                label: '香蕉',
                value: 'b',
                status: false
              },
              {
                type: 1,
                label: '桃子',
                value: 'c',
                status: false
              },
              {
                type: 1,
                label: '李子',
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
          }
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
      checkboxChange(data, name, isTextarea) {
        const question = this.questionData[name];
        const index = question.data.findIndex(item => item.value === data.value);
        if (isTextarea) {
          question.data[index].status = true;
        } else {
          question.data[index].status = !question.data[index].status;
        }
      },
      changeTextarea(data, name) {
        const question = this.questionData[name];
        question.data = data;
      },
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
