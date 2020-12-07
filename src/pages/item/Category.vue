<template>
  <v-card>
      <v-flex xs12 sm10>
        <v-tree ref="tree" url="/item/category/list"

                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>
  import {treeData} from "../../mockDB";

  export default {
    name: "category",
    data() {
      return {
        isEdit:true,
        treeData
      }
    },
    methods: {
      //新增节点
      handleAdd(node) {
        console.log("add .... ");
        console.log(node);

        this.$http({
          method: 'post',
          url: '/item/category',
          contentType:"application/json",
          data: this.$qs.stringify(node)
        }).then(() => {
          // 关闭窗口
          //this.$emit("close");
          this.reloadData(node.id);
          this.$message.success("保存成功！");
        })
          .catch(() => {
            this.$message.error("保存失败！");
          });
      },
      handleEdit(id, name) {
        console.log("edit... id: " + id + ", name: " + name)
      },
      handleDelete(id) {
        console.log("delete ... " + id);
        this.$http.delete("/item/category/cid/"+id).then(()=>{
          this.$message.success("删除成功!");
        }).catch(()=>{
          this.$message.error("删除失败!")
        })
      },
      handleClick(node) {
        console.log(node)
      },
      reloadData(id){
        this.$http.get("/item/category/list?pid="+1).then().catch();
      }
    }
  };
</script>

<style scoped>

</style>
