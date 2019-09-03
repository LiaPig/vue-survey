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
