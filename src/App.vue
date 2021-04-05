<template>
  <div id="app">
    <div class="content">
      <form class="form-inline" @submit="onSubmit">
        <input
          type="text"
          class="form-control mb-3 mr-sm-3"
          placeholder="Enter letters..."
          name="search"
        />
        <input class="btn btn-info mb-3" type="submit" value="Submit" />
      </form>
    </div>

    <p>
      <b>{{ results }}</b>
    </p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      data: [],
      results: "",
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      let str = event.target.elements.search.value;
      let data = this.data;
      let generatedArray = this.generateWords(str);
      let reqArray = generatedArray.filter((str) => data.includes(str));
      this.results = reqArray.join(", ");
    },
    init() {
      fetch(
        "https://gist.githubusercontent.com/adamc00/a45beb8a0cb55593220f749838c534d0/raw/fd11dcce5df4098839e19f21c50cc2363b9b7863/words.txt"
      )
        .then((response) => response.text())
        .then((data) => {
          let wordList = data.split("\n");
          this.data = wordList;
        });
    },
    generateWords(string, word = "", words = []) {
      if (!string) {
        words.push(word);
        return;
      }
      for (let i = 0; i < string.length; i++) {
        word += string[i];
        this.generateWords(
          string.slice(0, i) + string.slice(i + 1),
          word,
          words
        );
        word = word.slice(0, word.length - 1);
      }

      return [...new Set(words)];
    },
  },
  created() {
    this.init();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
}

.content {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
