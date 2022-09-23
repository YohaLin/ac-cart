<template>
  <div id="btn-container-1" class="control-container">
    <div
      :class="['btn-step', 'btn-last-step', { hidden: currentStep === 1 }]"
      @click.stop.prevent="minusStep"
    >
      ← 上一步
    </div>
    <template>
      <!-- 在最後一步時，文字有改變，用v-if條件渲染 -->
      <div
        class="btn-step btn-next-step"
        @click.stop.prevent="addStep"
        v-if="currentStep < steps.length"
      >
        下一步 →
      </div>
      <div
        class="btn-step btn-next-step"
        v-else
        @click.stop.prevent="{addStep,finishForm}"
      >
        完成下單
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: 'StepControl',
  props:{
    steps:{
      type: Array,
      required: true
    },
    currentStep:{
      type: Number,
      default: 1 // 預設為１
    }
  },
  methods: {
    addStep(){
      this.$emit('after-add-step')
      console.log('add')
    },
    minusStep(){
      this.$emit('after-minus-step')
    },
    finishForm(){
      this.$emit('after-finish-form')
    }
  }
}
</script>


<style scoped lang="scss">
@import "../assets/main.scss";
#btn-container-1 {
  height: 3rem;
  width: 100%;
  display: flex;
  margin: 1.5rem auto 0 auto;
  justify-content: space-between;
  .btn-step {
    cursor: pointer;
    width: 11rem;
    line-height: 3rem;
    border-radius: 0.5rem;
  }
  .btn-last-step {
    color: var(--number);
    text-align: left;
  }
  .btn-next-step {
    color: var(--white);
    background-color: var(--next-step);
    text-align: center;
    justify-self: self-end;
  }
  .hidden {
    visibility: hidden; // 使按鈕隱藏且不佔空間
  }
}
</style>