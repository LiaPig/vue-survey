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
