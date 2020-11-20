<template>
  <div id="app">
    <h1>lazy-cascader</h1>
    <div class="demo">
      <div class="demo-title">单选</div>
      <div>
        <lazy-cascader
          v-model="value"
          filterable
          :props="props"
        ></lazy-cascader>
      </div>
    </div>
    <div class="demo">
      <div class="demo-title">单选不关联</div>
      <div>
        <lazy-cascader
          v-model="value3"
          placeholder="请选择类目啊"
          width="500px"
          filterable
          :props="props3"
        ></lazy-cascader>
      </div>
    </div>
    <div class="demo">
      <div class="demo-title">多选</div>
      <div>
        <lazy-cascader
          v-model="value2"
          filterable
          :props="props2"
        ></lazy-cascader>
      </div>
    </div>
    <div class="demo">
      <div class="demo-title">多选不关联</div>
      <div>
        <lazy-cascader
          v-model="value4"
          filterable
          :props="props4"
        ></lazy-cascader>
      </div>
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
      current: [],
      options: [
        {
          id: 1,
          name: "服装",
          children: [
            {
              id: 3,
              name: "男装",
              children: [
                {
                  id: 7,
                  name: "T恤"
                }
              ]
            },
            {
              id: 4,
              name: "女装"
            }
          ]
        },
        {
          id: 2,
          name: "数码",
          children: [
            {
              id: 5,
              name: "手机"
            },
            {
              id: 6,
              name: "电脑"
            }
          ]
        }
      ],
      value: [1, 3, 7],
      props: {
        multiple: false,
        checkStrictly: false,
        value: "id",
        label: "name",
        leaf: "leaf",
        lazyLoad: this.lazyLoad,
        lazySearch: this.lazySearch
      },
      value2: [],
      props2: {
        multiple: true,
        checkStrictly: false,
        value: "id",
        label: "name",
        leaf: "leaf",
        lazyLoad: this.lazyLoad,
        lazySearch: this.lazySearch
      },
      value3: [],
      props3: {
        multiple: false,
        checkStrictly: true,
        value: "id",
        label: "name",
        leaf: "leaf",
        lazyLoad: this.lazyLoad,
        lazySearch: this.lazySearch
      },
      value4: [],
      props4: {
        multiple: true,
        checkStrictly: true,
        value: "id",
        label: "name",
        leaf: "leaf",
        lazyLoad: this.lazyLoad,
        lazySearch: this.lazySearch
      }
    };
  },
  methods: {
    //加载级联的方法
    lazyLoad(nodeValue, resolve) {
      setTimeout(() => {
        resolve(this.getCateList(nodeValue));
      }, 200);
    },
    lazySearch(queryString, callback) {
      setTimeout(() => {
        callback([
          {
            id: [1, 3, 7],
            name: ["服装", "男装", "T恤"]
          },
          {
            id: [1, 4],
            name: ["服装", "女装"]
          },
          {
            id: [2, 5],
            name: ["数码", "手机"]
          }
        ]);
      }, 200);
    },
    //获取节点数据
    getCateList(parent) {
      if (parent == 0) {
        return this.options.map(item => {
          let obj = {
            id: item.id,
            name: item.name,
            leaf: true
          };
          if (item.children && item.children.length > 0) {
            obj.leaf = false;
          }
          return obj;
        });
      } else {
        this.current = [];
        this.findCate(parent, this.options);
        return this.current.map(item => {
          let obj = {
            id: item.id,
            name: item.name,
            leaf: true
          };
          if (item.children && item.children.length > 0) {
            obj.leaf = false;
          }
          return obj;
        });
      }
    },
    findCate(parent, children) {
      for (let i = 0; i < children.length; i++) {
        if (parent == children[i].id) {
          this.current = children[i].children;
        } else {
          if (children[i].children) {
            this.findCate(parent, children[i].children);
          }
        }
      }
    }
  }
};
</script>

<style lang="less">
#app {
  width: 800px;
  margin: 100px auto;
}
.demo {
  margin-top: 10px;
}
</style>
