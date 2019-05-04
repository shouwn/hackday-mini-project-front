<template>
  <div>
    <div
      v-for="word in words"
      :key="word"
      v-html="highlight(word)"></div>
  </div>
</template>

<script>
  export default {
    props: {
      words: {
        type: Array,
        required: true,
        description: "화면에 표시될 단어들"
      },
      keyword: {
        type: String,
        required: true,
        description: "강조 표시할 키워드"
      }
    },
    methods: {
      highlight(text) {
        return text.trim().replace(this.regex,
          "<span style='color: mediumseagreen'>" + this.keyword + "</span>");
      }
    },
    computed: {
      regex() {
        return new RegExp(this.keyword, 'g');
      }
    }
  }
</script>

<style scoped>

</style>
