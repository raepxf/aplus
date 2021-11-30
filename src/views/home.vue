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
  <van-grid-item v-for="value in 6" :key="value" :text="'文字'+value" :class="['typeChildren', value === currentIndex ? 'active': '']"  @touchstart.stop="currentIndex = value" />
</van-grid>
 <h5>二级类型</h5>
        <van-grid :column-num="5" square :gutter="10">
  <van-grid-item v-for="value in 6" :key="value" text="文字" class="typeChildren" />
</van-grid>
<h5>支付方式</h5>
        <van-grid :column-num="5" square :gutter="10">
  <van-grid-item v-for="row in payList" :key="row.value" :text="row.label" class="typeChildren" />
</van-grid>
<van-field
  v-model="result"
  is-link
  readonly
  name="picker"
  label="选择时间"
  placeholder="点击选择时间"
  @touchstart.stop="showTimes = true"
/>

<van-action-sheet v-model:show="showTimes" title="标题">
  <van-datetime-picker
  v-model="result"
  type="datetime"
  title="选择完整时间"
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
import { defineComponent, reactive, ref } from 'vue'

export default defineComponent({
    setup() {
      const active = ref(0);
      const show = ref(false)
      const value = ref('')
      const currentIndex = ref(1)
      const forms = reactive({
       content: "",
       createTime: "",
       paymentMethod: 0,
       price: "",
       remark: "",
       typeId: "",
       typeParentId: "",
      })
      const checked = ref('1')
      const result = ref('')
      const showTimes = ref(false)
      const payList = ref([
        { label: '支付宝', value: '1'},
        { label: '微信', value: '2'},
        { label: '银行卡', value: '3'},
        { label: '现金', value: '4'},
        { label: '地铁卡', value: '5'}
      ])
    return { active, value, show, forms, currentIndex, checked, payList, result, showTimes }
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

