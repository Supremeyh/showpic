
<template>
  <div class="picbox">
    <p>排序方式: 
      <el-button @click="toggoleShow('time')"> 时间 </el-button>
      <el-button @click="toggoleShow('cate')"> 分类 </el-button>
    </p>

    <!-- 按时间 -->
    <div class='box' v-if="sort!=='cate'">
      <ul>
        <template  v-for="item in filterPicListTime">
          <li v-if="item.name" :key="item.id" class='pic'>
            <p>名称: {{item.name}}</p>
            <p>分类: {{item.cate}}</p>
            <p>时间: {{item.time}}</p>
          </li>
        </template>
      </ul>
    </div>

    <!-- 按分类 -->
    <div class='box' v-else>
      <el-tree
        :data="filterPicListCate"
        :props="defaultProps" 
        node-key="id"
        highlight-current
        default-expand-all
        draggable
        @node-drop="handleDrop"
        :allow-drop="allowDrop"
        :allow-drag="allowDrag"
        >
        <span class="custom-tree-node" slot-scope="{node,data}">
          <span>{{data.cate}}</span>
          <span v-if="data.name"> - 名称{{data.name}}</span>
          <span v-if="data.time"> - 时间{{data.time}}</span>
        </span>
      </el-tree>
    </div>

  </div>
</template>

<script>
import { pipeline } from 'stream';

let picList = [
  { cate: 'A', children: [{id: 1, name: '1', cate: 'A', time: '1'}] },
  { cate: 'B', children: [{id: 2, name: '1', cate: 'B', time: '4'}, {id: 3, name: '3', cate: 'B', time: '3'}] },
  { cate: 'C', children: [{id: 4, name: '4', cate: 'C', time: '2'}] },
  { cate: 'D', children: [{id: 5, name: '5', cate: 'D', time: '8'}] },
  { cate: 'E', children: [{id: 6, name: '6', cate: 'E', time: '4'}] },
  { cate: 'F', children: [
    {id: 7, name: '7', cate: 'F', time: '2'}, 
    {id: 8, name: '8', cate: 'F', time: '5'}, 
    {id: 9, name: '9', cate: 'F', time: '4'}, 
    {id: 10, name: '10', cate: 'F', time: '5'},
    {id: 11, name: '11', cate: 'F', time: '25'},
    {id: 12, name: '12', cate: 'F', time: '5'},
    {id: 13, name: '13', cate: 'F', time: '5'},
    {id: 14, name: '14', cate: 'F', time: '12'},
    {id: 15, name: '15', cate: 'F', time: '9'},
    {id: 16, name: '16', cate: 'F', time: '4'},
    {id: 17, name: '17', cate: 'F', time: '3'},
    {id: 18, name: '18', cate: 'F', time: '18'},
    {id: 19, name: '19', cate: 'F', time: '10'},
    {id: 20, name: '20', cate: 'F', time: '13'},
    {id: 21, name: '21', cate: 'F', time: '4'},
    ] },
    { cate: 'G', children: [{id: 22, name: '22', cate: 'G', time: '1'}] },
]

export default {
  name: 'PicBox',
  data() {
    return {
      sort: '',
      filterPicListTime: picList,
      filterPicListCate: picList,
      cateArr: [],
      defaultProps: {
        label: 'cate',
        children: 'children'
      }
    }
  },
  methods: {
    toggoleShow(val) {
      this.sort = val
      val==='time'? this.showByTime() : null
    },
    showByTime() {
      let nodes =  picList
      let obj = {}
      let arr = []
      for(let i=0; i< nodes.length; i++) {
        let node = nodes[i]
        if(node.children && node.children.length>0) {
          for(let j=0; j< node.children.length; j++) {
            arr.push(node.children[j])
          }
        }
      }

      this.filterPicListTime = arr.sort((a, b) => {
        return a.time - b.time
      })
    },
    allowDrag(node) {
      return node.level===2 ? true : false
    },
    allowDrop(draggingNode, dropNode, type) {      
      if(dropNode.level===1 && (type ==='inner')) {
          return true
        }
      return false
    },
     handleDrop(draggingNode, dropNode, dropType) {
       draggingNode.data.cate = dropNode.data.cate
      }
    }
}
</script>


<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.cate{
  width: 300px;
  height: 30px;
  border: 1px solid #42b983;
  display: block;
  margin: 10px auto;
}
.pic{
  float: left;
  width: 120px;
  height: 120px;
  display: block;
  border: 1px solid #ccc;
  cursor: pointer;
}
</style>
