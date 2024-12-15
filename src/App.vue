<script>
import { ref, onMounted, onBeforeUnmount, useModel } from 'vue';
import Typed from 'typed.js';
import axios from 'axios';

export default {
  name: 'MyComponent',
  methods: {
    goTwitter() {
      window.open('https://x.com/_dream_ai_sol');
    },
    goPump() {
      window.open('https://pump.fun/');
    },
    getData() {
      const { loading } = this.$data;
      if (loading) {
        return;
      }
      this.$data.loading = true;
      this.$data.hit = 'Waiting.....';
      const api_key = 'sk-eNPh4ZGTUIU3LsCj8M6SVGhlQi5BSQYU0WUoLn9xo06WkbcA';
      let data = JSON.stringify({
        model: 'theb-ai',
        messages: [
          {
            role: 'user',
            content: `Analyze the dream I had last night: ${this.inputValue}. Use the official language to explain it. Four or five sentences will do.`,
          },
        ],
        stream: false,
      })

      let config = {
        method: 'post',
        maxBodyLength: Infinity,
        url: 'https://api.theb.ai/v1/chat/completions',
        // url: 'https://api.baizhi.ai/v1/chat/completions',
        headers: {
          Authorization: `Bearer ${api_key}`,
          'Content-Type': 'application/json',
        },
        data: data,
      }
      const that = this;
      axios
        .request(config)
        .then((response) => {
          const result = response.data?.choices[0].message?.content;
          // 循环从第一个字母开始，每次加一个长度，赋值给data.result
          that.$data.result = result;
          this.$data.loading = false;
          this.$data.hit = 'Type your dream,and confirm.';
        })
        .catch((error) => {
          console.log(error)
          this.$data.loading = false;
          this.$data.hit = 'Type your dream,and confirm.';
        })
    }
  },
  data() {
    return {
      result: 'none',
      hit: 'Type your dream,and confirm.',
      loading: false,
      inputValue: ''
    }
  },
  setup() {
    // 创建引用来存储包含动画的DOM元素
    const el = ref(null);

    onMounted(() => {
      
      const typed = new Typed(el.value, {
        strings: [
          'I will help you analyze your dreams.',
          'Your dreams hold clues to your reality.',
          'Every dream has a hidden message for you.',
          'Unravel the mystery of your dreams with my analysis.',
          'Discover the connection between dreams and your daily life.',
          'Let me interpret your dreams and unveil their meanings.',
          'Dreams bridge the gap between fantasy and reality.',
          'The secrets of your dreams can reveal real-life insights.',
          'Decode the symbols in your dreams to understand your life.',
          'Explore how your dreams relate to your waking world.'
        ],
        typeSpeed: 50,
      });

      // 在组件销毁前清理Typed实例
      onBeforeUnmount(() => {
        typed.destroy();
      });
    });

    return { el };
  },
};
</script>

<template>
  <div id="app">
    <img class="banner" src="/banner.jpg"/>
    <div class="font">
      <span ref="el"></span>
    </div>
    <div class="input-area">
      <input type="text" v-model="inputValue" class="input" />
      <div v-on:click="getData" class="button">Confirm</div>
    </div>
    <div class="font">{{ hit }}</div>
    <div class="result">{{result}}</div>
    <div>
      <img class="icon" v-on:click="goPump" src="/pump.jpg" />
    </div>
  </div>
</template>

<style scoped>
.icon {
  width: 30px;
  height: 30px;
  margin: 10px;
}
.icon:hover {
  cursor: pointer;
}
.result {
  width: 800px;
  min-height: 100px;
  border: 2px solid #007002;
  text-align: left;
  color: #007002;
  font-size: 18px;
}
.font {
  color: #007002;
  font-size: 18px;
  font-weight: 500;
}
.banner {
  width: 800px;
}
.input-area {
  margin-top: 20px;
}
.input {
  width: 400px;
  height: 32px;
  font-size: 18px;
  color: #007002;
  background: #000;
  border-radius: 5px;
  border: 2px solid #007002;
}
.button {
  cursor: pointer;
  display: inline-block;
  width: 100px;
  height: 34px;
  margin-left: 8px;
  line-height: 34px;
  font-size: 18px;
  color: #007002;
  background: #000;
  border-radius: 5px;
  border: 2px solid #007002;
}
.button:hover {
  background: #007002;
  color: #000;
}
</style>
