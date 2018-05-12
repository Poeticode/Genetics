<template>
  <div id="wrapper">
    <main>
      <div class="title">
        <span>
          Genetics - A highly configurable password generator
        </span>
      </div>
      <div class="generator">
          <word-reader></word-reader>
      </div>
    </main>
  </div>
</template>

<script>
  import WordReader from './WordReader'

  export default {
    name: 'landing-page',
    components: { WordReader },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      }
    }
  }
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  #wrapper {
    background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;
  }

  main {
    display: flex;
    justify-content: space-between;
  }

  .title span {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
    &.alt {
      font-size: 18px;
      margin-bottom: 10px;
    }
  }
  .generator {
    p {
      color: black;
      margin-bottom: 10px;
    }
    button {
      font-size: .8em;
      cursor: pointer;
      outline: none;
      padding: 0.75em 2em;
      border-radius: 2em;
      display: inline-block;
      color: #fff;
      background-color: #4fc08d;
      transition: all 0.15s ease;
      box-sizing: border-box;
      border: 1px solid #4fc08d;
    }
    button.alt {
      color: #42b983;
      background-color: transparent;
    }
  }
</style>
