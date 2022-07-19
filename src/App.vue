<template>
  <div id="app">
    <header-view></header-view>
    <chat-content></chat-content>
    <footer-view></footer-view>
  </div>
</template>

<script>
import ChatContent from "./components/Chat-content.vue";
import HeaderView from "./components/Header.vue";
import FooterView from "./components/Footer.vue";
export default {
  name: "App",
  components: {
    HeaderView,
    FooterView,
    ChatContent,
  },
};
</script>
