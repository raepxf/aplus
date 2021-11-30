<template>
    <div class="layout">
     <div class="container">
     <div >
<van-field v-model="value" label="金额(￥)" readonly clickable @touchstart.stop="show = true" placeholder="0.00"/>
<van-number-keyboard
  v-model="value"
  :show="show"
  extra-key="."
  :maxlength="6"
  @blur="show = false"
/>
        </div>
        <h5>一级类型</h5>
        <van-grid :column-num="5" square :gutter="10">
  <van-grid-item v-for="row in typeList" :key="row.typeId" :text="row.typeName" :class="['typeChildren', row.typeId === currentIndex ? 'active': '']"  @touchstart.stop="currentIndex = row.typeId" />
</van-grid>
 <h5>二级类型</h5>
        <van-grid :column-num="5" square :gutter="10">
  <van-grid-item v-for="row in typeChildrenList" :key="row.value" :text="row.typeName" :class="['typeChildren', row.typeId === currentCIndex ? 'active': '']"  @touchstart.stop="currentCIndex = row.typeId" />
</van-grid>
<h5>支付方式</h5>
        <van-grid :column-num="5" square :gutter="10">
  <van-grid-item v-for="row in payList" :key="row.value" :text="row.label" :class="['typeChildren', row.value === forms.paymentMethod ? 'active': '']"  @touchstart.stop="forms.paymentMethod = row.value" />
</van-grid>
<van-field
  v-model="forms.createTime"
  is-link
  readonly
  name="picker"
  label="选择时间"
  placeholder="点击选择时间"
  @click="showTimes = true"
/>

<van-action-sheet v-model:show="showTimes" title="标题">
  <van-datetime-picker
  v-model="result"
  type="datetime"
  title="选择完整时间"
  @confirm="onConfirm"
  @cancel="showTimes = false"
/>
</van-action-sheet>
<van-field v-model="forms.content" label="内容" placeholder="内容"/>
<van-field v-model="forms.remark" label="备注" placeholder="备注"/>
     </div>

        <van-tabbar v-model="active">
  <van-tabbar-item icon="home-o">首页</van-tabbar-item>
  <van-tabbar-item icon="location-o">发现</van-tabbar-item>
  <van-tabbar-item icon="eye-o">生活</van-tabbar-item>
  <van-tabbar-item icon="user-o">我的</van-tabbar-item>
</van-tabbar>
    </div>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

export default defineComponent({
    setup() {
      const active = ref(0);
      const show = ref(false)
      const value = ref('')
      const currentIndex = ref(1)
      const forms = reactive({
       content: "",
       createTime: "",
       paymentMethod: 1,
       price: "",
       remark: "",
       typeId: "",
       typeParentId: "",
      })
      const checked = ref('1')
      const result = ref(new Date)
      const showTimes = ref(false)
      const currentCIndex = ref(1)
      const payList = ref([
        { label: '现金', value: 1 },
        { label: '支付宝', value: 2 },
        { label: '微信', value: 3 },
        { label: '银行卡', value: 4 },
        { label: '地铁卡', value: 5 }
      ])
      const typeList: any = ref([])
      const typeChildrenList: any = ref([])
      const getTimeStr =  (strs: any, type = '-') => {
    const timeObj = new Date(strs)
    const yy = timeObj.getFullYear()
    const mm = timeObj.getMonth() < 9 ? ('0' + (timeObj.getMonth() + 1)) : (timeObj.getMonth() + 1)
    const dd = timeObj.getDate() > 9 ? timeObj.getDate() : ('0' + timeObj.getDate())
    const hh = timeObj.getHours() < 10 ? ('0' + timeObj.getHours()) : timeObj.getHours()
    const min = timeObj.getMinutes() < 10 ? ('0' + timeObj.getMinutes()) : timeObj.getMinutes()
    const sec = timeObj.getSeconds() < 10 ? ('0' + timeObj.getSeconds()) : timeObj.getSeconds()
    return `${yy}${type}${mm}${type}${dd} ${hh}:${min}:${sec}`
  }
      const onConfirm = (value: any) => {
        result.value = value
        forms.createTime = getTimeStr(value)
        showTimes.value = false
      }
      const convertToTreeData = (data: any, pid: string) => {
      const result = []
      let temp = []
      for (let i = 0; i < data.length; i++) {
        if (data[i].typeParentId === pid) {
            const obj = data[i]
            temp = convertToTreeData(data, data[i].typeId)
            if (temp.length > 0) {
              obj.children = temp
            }
            result.push(obj)
          }
      }
      return result
    }
    onMounted(async () => {
        const res = await axios.get('http://124.71.192.159/api/expenses/getType', {})
        typeList.value = convertToTreeData(res.data.data, '')
        currentIndex.value = typeList.value[0].typeId
    })
    watch(currentIndex, (val) => {
      typeChildrenList.value = typeList.value.filter((r:any) => r.typeId === val)[0].children
    })
    return { active, value, show, forms, currentIndex, checked, payList, result, showTimes, typeList, typeChildrenList, currentCIndex, onConfirm }
    },
})
</script>

<style>
.layout{
    width:100%;
  height: 100%;
}
.container{
    width:100%;
    height: calc(100% - 50px);
overflow: auto;
/* display: none; */
}
.typeChildren>div{
    border-radius: 50%;
    background: #f6f8fa;
}
.typeChildren.active>div{
    background: #1989fa;
}
.typeChildren.active>div .van-grid-item__text{
    color:#fff
}
</style>

