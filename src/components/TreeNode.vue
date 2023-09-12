<template>
  <div class="tree-node">
    <div class="d-flex align-items-center">
      <span v-if="childrenLength" @click="toggleNode" class="cursor-pointer">{{ isOpen ? '🔼' : '🔽' }}</span>
      <span class="node-text">{{ node.text }}</span>
      <button class="btn btn-success btn-sm ms-2" @click="addChildNode">Add node</button>
      <button v-if="node.text !== 'Root'" class="btn btn-danger btn-sm ms-2" @click="removeNode">Remove node</button>
    </div>
    <div class="branch" v-if="isOpen && childrenLength">
      <tree-node v-for="(childNode, index) in node.children" :key="index" :node="childNode" @updateTree="updateTree" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    node: Object,
  },
  data() {
    return {
      isOpen: true,
    };
  },
  methods: {
    addChildNode() {
      // eslint-disable-next-line vue/no-mutating-props
      this.node.children.push({ text: "New node", children: [] });
      this.isOpen = true;
      this.updateTree();
    },
    removeNode() {
      const parentNode = this.$parent.node;
      const index = parentNode.children.indexOf(this.node);
      if (index !== -1) {
        parentNode.children.splice(index, 1);
        this.updateTree();
      }
    },
    toggleNode() {
      this.isOpen = !this.isOpen;
    },
    updateTree() {
      this.$emit('updateTree', this.node);
    },
  },
  computed: {
    childrenLength() {
      return this.node.children.length > 0
    }
  },
  watch: {
    'node': 'updateTree',
  },
};
</script>

<style scoped>
.tree-node {
  margin-left: 40px;
  margin-top: 10px;
  position: relative;
}

.node-text {
  margin-left: 10px;
  cursor: default;
}

.cursor-pointer {
  cursor: pointer;
  position: absolute;
  transform: matrix(1, 0, 0, 1, -20, 0);
}

.branch::before {
  content: '';
  position: absolute;
  top: 28px;
  left: -10px;
  width: 15px;
  height: calc(100% - 28px);
  z-index: -1;
  border-left: 1px solid #000;
  border-bottom: 1px solid #000;
}
</style>