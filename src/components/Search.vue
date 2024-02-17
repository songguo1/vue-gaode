<template>
  <div id="search">
    <el-input
      type="text"
      placeholder="请输入内容"
      v-model="userInput"
      :id="inputId"
      clearable
      @input="send"
    ></el-input>
  </div>
</template>

<script>
import { EventBus } from "../eventbus/event-bus";
export default {
  data() {
    return {
      userInput: "",
      inputId: "seachInput",
    };
  },
  methods: {
    send() {
      EventBus.$emit("shareinputId", this.inputId);
    },
  },
  mounted(){
    this.send();//初始化，让其他组件知道这个搜索输入框组件已经被加载并准备好接受用户输入了。
  },
};
</script>

<style>
#search {
  position: relative;
  margin-left: auto;
  margin-right: auto;
  top: 30px;
  width: 600px;
}
</style>
