<template>
  <div class="about">
    <h1>This is parent home page</h1>
    <a-button @click="openDrawer"> 主应用打开抽屉加载微应用</a-button>
    <div class="container-wrap">
      <div id="appContainer1"></div>
      <div id="appContainer2"></div>
    </div>

    <a-drawer
      title="测试弹框加载微应用"
      placement="right"
      width="60%"
      :visible="visible"
      :after-visible-change="afterVisibleChange"
      @close="onClose"
    >
      <div id="appContainer3"></div>
    </a-drawer>
  </div>
</template>

<script>
import { loadMicroApp } from "qiankun";
import { Button, Drawer } from "ant-design-vue";
export default {
  components: { [Button.name]: Button, [Drawer.name]: Drawer },
  data() {
    return {
      visible: false,
    };
  },
  mounted() {
    loadMicroApp({
      name: "app-vue-hash",
      entry: "http://localhost:1111",
      container: "#appContainer1",
      props: { data: { defaultPath: "/about" } },
    });
    loadMicroApp({
      name: "app-vue-hash",
      entry: "http://localhost:2222",
      container: "#appContainer2",
    });
  },
  methods: {
    afterVisibleChange(val) {
      console.log("visible", val);
    },
    openDrawer() {
      this.visible = true;
      // loadMicroApp放在这里，是因为调用loadMicroApp的时候，必须保证container节点已经存在于页面上
      loadMicroApp({
        name: "app-vue-hash333",
        entry: "http://localhost:1111",
        container: "#appContainer3",
        props: { data: { defaultPath: "/", drawer: "333" } },
      });
    },
    onClose() {
      this.visible = false;
    },
  },
};
</script>

<style scoped>
h1 {
  color: green;
}
.container-wrap {
  width: 1200px;
  margin: 0 auto;
  display: flex;
  height: 710px;
}
#appContainer1 {
  width: 600px;
  background: #f8b4b4;
}
#appContainer2 {
  width: 600px;
  background: #d3f8b4;
}
</style>
