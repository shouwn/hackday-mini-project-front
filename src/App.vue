<template>
  <div id="app">
    <search-options-view
      :options="options"
      v-model.lazy="currentOption"
    />
    <div class="search">
      <input
        v-model="keyword"
        placeholder="검색어를 입력해주세요."
      />
      <text-view
        :words="words"
        :keyword="keyword"
      />
    </div>
    평균 시간: {{average || 0}} ms
    <div class="time">
      <div v-for="time in times">
        {{time}} ms
      </div>
    </div>
  </div>
</template>

<script>
  import SearchOptionsView from "@/components/SearchOptionsView";
  import TextView from "@/components/TextView";

  import * as _ from "lodash";

  const options = ["startsWith", "contains", "endsWith"];

  export default {
    components: {
      TextView,
      SearchOptionsView
    },
    data() {
      return {
        options: options, // 옵션 목록
        currentOption: options[0], // 현재 선택 중인 옵션
        words: [], // 검색 결과
        keyword: "", // 검색어
        times: [] // 서버 응답 시간들을 저장하고 있는 배열
      }
    },
    methods: {
      /**
       * 서버에서 자동 완성 요청을 보내는 메소드
       */
      search: _.debounce(function (keyword, option) {
        if (keyword) {
          let startTime = new Date().getTime();

          this.$http.get(`/item/title/${option}/${keyword}/size/5`)
            .then(result => {
              this.words = result.data.map(item => item.title);

              this.times.push(new Date().getTime() - startTime);
            })
        } else {
          this.words = [];
        }
      }, 200)
    },
    computed: {
      average() {
        let times = [...this.times];
        let average = times.reduce((sum, cur) => sum + cur, 0) / times.length;

        return Math.round(average);
      }
    },
    watch: {
      currentOption(newOption) {
        this.search(this.keyword, newOption);
      },
      keyword(newKeyword) {
        this.search(newKeyword, this.currentOption);
      }
    }
  }
</script>

<style>
  html {
    display: flex;
    flex-direction: column;
    height: 100vh;
  }

  body {
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  #app {
    display: flex;
    flex-direction: column;
    flex: 1;
  }

  * {
    font-size: 1.3rem
  }

  .search {
    display: flex;
    flex-direction: column;

    margin-top: 0.5rem;
    margin-bottom: 0.5rem;
  }

  .search > * {
    border: black solid 0.1rem;
    padding: 0.2rem;
  }

  .search > *:last-child {
    margin-top: 0.3rem;
  }

  .time {
    border: black solid 0.1rem;
    padding: 0.2rem;

    flex: 1;
    overflow: auto;
  }
</style>
