<template>
  <div class="options">
    <checkbox
      class="options__checkbox"
      v-for="option in options"
      :key="option"
      :label="option"
      :active="isActive(option)"
      @toggle="onToggle"
    />
  </div>
</template>

<script>
  import Checkbox from "@/components/Checkbox";

  export default {
    components: {
      Checkbox
    },
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
      onToggle(label, checked) {
        let value = "";

        if (checked) {
          value = label;
        }

        this.$emit('input', label);
      },
      isActive(label) {
        return this.value === label;
      }
    }
  }
</script>

<style scoped>
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
