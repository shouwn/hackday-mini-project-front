<template>
  <div>
    <label>
      <input type="checkbox" :id="'$_checkbox_' + label" :checked="active" @click="onClick"/>
      <label :for="'$_checkbox_' + label"></label>
      {{label}}
    </label>
  </div>
</template>

<script>
  export default {
    props: {
      label: {
        type: String,
        required: true,
        description: "체크 박스 옆에 표시될 글씨"
      },
      active: {
        required: true,
        description: "체크박스 체크 여부"
      }
    },
    methods: {
      onClick() {
        this.$emit('toggle', this.label, !this.active);
      }
    }
  }
</script>

<style scoped>
  input {
    display: none;
  }

  input + label {
    background-color: #fafafa;
    border: 1px solid #cacece;
    padding: 9px;
    top: 4px;
    border-radius: 3px;
    display: inline-block;
    position: relative;
  }

  input + label:active, input:checked + label:active {
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px 1px 3px rgba(0, 0, 0, 0.1);
  }

  input:checked + label {
    background-color: #e9ecee;
    border: 1px solid #adb8c0;
    color: #99a1a7;
  }

  input:checked + label:after {
    content: '\2714';
    font-size: 14px;
    position: absolute;
    top: 0;
    left: 3px;
    color: black;
  }
</style>
