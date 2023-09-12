<template>
    <div class="container">
        <button class="btn btn-success btn-sm ms-2" :class="{ disabled: isDisabled }" @click="createRoot">Создать
            корень</button>
        <button v-if="rootNode" class="btn btn-danger btn-sm ms-2" @click="removeRoot">Удалить корень</button>

        <tree-node class="root" v-if="rootNode" :node="rootNode" @updateTree="updateTree" />
    </div>
</template>
  
<script>
import TreeNode from './TreeNode.vue';

export default {
    components: {
        TreeNode,
    },
    data() {
        return {
            rootNode: null,
        };
    },
    methods: {
        createRoot() {
            this.rootNode = { text: 'Корень', children: [] };
            this.saveTreeToLocalStorage();
        },
        removeRoot() {
            this.rootNode = null;
            this.saveTreeToLocalStorage();
        },
        updateTree(treeData) {
            this.rootNode = treeData;
            this.saveTreeToLocalStorage();
        },
        saveTreeToLocalStorage() {
            localStorage.setItem('treeData', JSON.stringify(this.rootNode));
        },
    },
    computed: {
        isDisabled() {
            return this.rootNode
        }
    },
    created() {
        const storedData = localStorage.getItem('treeData');
        if (storedData) {
            this.rootNode = JSON.parse(storedData);
        }
    },
};
</script>

