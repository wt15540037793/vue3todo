<template>
  <div>
    <!-- {{list}} -->
    <input type="text" v-model="user.text" @keyup.enter="add" />
    <h3>
      正在进行(<span>{{ sum2 }}</span
      >)
    </h3>
    <ul>
      <li v-for="(item, index) in list" :key="index" v-show="!item.flag">
        <input type="checkbox" @click="change" v-model="item.flag" />
        <input type="text" v-show="item.isflag" @blur="edit" v-model="item.txt"/>
        <span v-show="!item.isflag" @dblclick="isEdit">{{item.text
        }}</span>
        <button @click="del(index)">删除</button>
      </li>
    </ul>
    <h3>已经完成({{ sum1 }})</h3>
    <ul>
      <li v-for="(item, index) in list" :key="index" v-show="item.flag">
        <input type="checkbox" @click="change" v-model="item.flag"
          @blur="edit"/>
        <input type="text" :value="item.text" v-show="item.isflag" />
        <span v-show="!item.isflag" @dblclick="isEdit">{{
          item.text
        }}</span>
        <button @click="del(index)">删除</button>
      </li>
    </ul>
  </div>
</template>
<script>
import { reactive, computed, ref, watch } from "vue";
export default {
  setup() {
    let user = reactive({
      text: "",
      txt:"",
      flag: false,
      isflag: false,
    });
    let list = reactive([]);
    let isEdit = reactive(false);
    let add = () => {
      if (user.text.length == "") {
        return alert("内容不能为空");
      }
      let obj = { ...user };
      list.push(obj);
      console.log(list);
      user.text = null;
    };
    let change = () => {
      user.flag = !user.flag;
    };
    let del = (index) => {
      list.splice(index, 1);
    };
    let sum1 = computed(() => {
      let num = 0;
      list.forEach((item) => {
        if (item.flag) {
          num++;
        }
      });
      return num;
    });
    let sum2 = computed(() => {
      let num = 0;
      list.forEach((item) => {
        if (!item.flag) {
          num++;
        }
      });
      return num;
    });
    let idEdit = (index) => {
      user.txt = user.text
      user.isflag = true
    }
    let edit = () => {
      user.isflag=false
    };
    return { user, add, list, change, del, isEdit, sum1, sum2, edit, idEdit };
  },
};
</script>
<style lang="scss">
* {
  padding: 0;
  margin: 0;
}
input{
  width: 200px;
  height: 20px;
  margin-top: 20px;
}
h3{
  margin-top: 20px;
}
div{
  text-align: center;
}
li {
  list-style: none;
  // background-color: aqua;
}
button{
  margin-left: 20px;
}
</style>
