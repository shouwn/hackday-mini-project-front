<template>
  <div class="options">
    <label
      class="options__checkbox"
      v-for="option in options"
      :key="option"
    >
      <input
        type="checkbox"
        :id="'$_checkbox_' + option"
        :checked="isActive(option)"
        :disabled="isActive(option)"
        @click="$emit('input', option)"
      />
      <!-- checkbox css 적용을 위한 label -->
      <label :for="'$_checkbox_' + option"></label>
      {{option}}
    </label>
  </div>
</template>

<script>
  export default {
    props: {
      options: {
        type: Array,
        required: true,
        description: "선택할 수 있는 옵션 목록"
      },
      value: {
        type: String,
        description: "v-model 용도"
      }
    },
    methods: {
      isActive(label) {
        return this.value === label;
      }
    }
  }
</script>

<style scoped>
  @import '../asserts/checkbox.css';

  .options {
    display: flex;
  }

  .options__checkbox {
    background-color: aliceblue;

    text-align: center;

    flex: 1;

    padding: 0.4rem;
    margin-right: 1rem;

    border: black solid 0.1rem;
    border-radius: 0.2rem;
  }

  .options__checkbox:last-child {
    margin-right: 0;
  }

</style>
