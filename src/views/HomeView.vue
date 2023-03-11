<template>
  <div class="mt-52">
    <div class="tree-wrap" style="height: 400px">
      <div class="search-box">
        <span>搜索：</span>
        <input
          type="text"
          v-model="state.keyword"
          placeholder="請輸入關鍵字"
          @keydown.enter="filter"
        />
      </div>
      <vue3TreeOrg
        ref="tree"
        :data="state.data"
        :horizontal="state.horizontal"
        :label-style="state.style"
        :node-draggable="true"
        :scalable="true"
        :only-one-node="state.onlyOneNode"
        :default-expand-level="1"
        :filter-node-method="filterNodeMethod"
        :clone-node-drag="state.cloneNodeDrag"
        @on-restore="restore"
        @on-contextmenu="onMenus"
        @on-node-click="onNodeClick"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue';

import vue3TreeOrg2 from '@/util/vue3-tree-org/index.esm';
import '@/util/vue3-tree-org/vue3-tree-org.css';
const tree = ref();
const Vue3TreeOrg = vue3TreeOrg2.Vue3TreeOrg;

const cavansStatus = reactive({
  screenshotData: null,
  screenshotWidth: 0,
  screenshotHeight: 0,
});

const state = reactive({
  data: {
    id: 1,
    label: 'xxx科技有限公司',
    children: [
      {
        id: 2,
        pid: 1,
        label: '研發部',
        style: { color: '#fff', background: '#108ffe' },
        children: [
          { id: 6, pid: 2, label: '禁止編輯', disabled: true },
          { id: 8, pid: 2, label: '禁止拖曳', noDragging: true },
          { id: 10, pid: 2, label: '測試' },
        ],
      },
      {
        id: 3,
        pid: 1,
        label: '客服部',
        children: [
          { id: 11, pid: 3, label: '客服一部' },
          { id: 12, pid: 3, label: '客服二部' },
        ],
      },
      { id: 4, pid: 1, label: '業務部' },
    ],
  },
  keyword: '',
  horizontal: false,
  collapsable: true,
  onlyOneNode: false,
  cloneNodeDrag: false,
  expandAll: true,
  style: {
    background: '#fff',
    color: '#5e6d82',
  },
});

const filterNodeMethod = (value, data) => {
  console.log(value, data);
  console.log('click');
  if (!value) return true;
  return data.label.indexOf(value) !== -1;
};

const restore = () => {
  console.log('restore');
};

const onMenus = ({ node, command }) => {
  console.log(node, command);
};
const filter = () => {
  tree.value.filter(state.keyword);
};

const onNodeClick = (e, data) => {
  console.log(e, data);
  // this.$Message.info(data.label);
};
</script>

<style>
.tree-wrap {
  position: relative;
  padding-top: 52px;
}

.search-box {
  padding: 8px 15px;
  position: absolute;
  top: 0;
  left: 0;
}
.search-box input {
  width: 200px;
  height: 32px;
  border: 1px solid #ddd;
  outline: none;
  border-radius: 5px;
  padding-left: 10px;
}
.tree-org-node__text {
  text-align: left;
  font-size: 14px;
}
.custom-content {
  padding-bottom: 8px;
  margin-bottom: 8px;
  border-bottom: 1px solid currentColor;
}

.tree-org-node__textarea {
  width: 200px;
  height: 100px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  line-height: 1.5;
}
.zoom-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
