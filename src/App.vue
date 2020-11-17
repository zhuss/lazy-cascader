<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <div class="demo">
      <lazy-cascader v-model="value" filterable :props="props"></lazy-cascader>
    </div>
    <div class="doc">
      <div class="doc-title"></div>
    </div>
  </div>
</template>

<script>
import LazyCascader from "./components/cascader/LazyCascader";
export default {
  name: "app",
  components: {
    LazyCascader
  },
  data() {
    return {
      value: [1, 2, 3, 4],
      props: {
        multiple: false,
        checkStrictly: false,
        value: "v",
        label: "l",
        leaf: "Leaf",
        lazyLoad: this.lazyLoad,
        lazySearch: this.lazySearch
      }
    };
  },
  methods: {
    //加载级联的方法
    lazyLoad(nodeValue, resolve) {
      if (nodeValue < 3) {
        setTimeout(() => {
          resolve([
            {
              v: nodeValue + 1,
              l: "类目" + (nodeValue + 1),
              Leaf: false
            }
          ]);
        }, 300);
      } else {
        setTimeout(() => {
          resolve([
            {
              v: nodeValue + 1,
              l: "类目" + (nodeValue + 1),
              Leaf: true
            }
          ]);
        }, 300);
      }
    },
    lazySearch(queryString, callback) {
      callback([
        {
          v: [1, 2, 3, 4],
          l: ["类目1", "类目2", "类目3", "类目4"]
        }
      ]);
    }
  }
};
</script>

<style lang="less"></style>
