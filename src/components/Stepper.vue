<template>
  <div class="stepper-container d-flex">
    <div 
    :class="['step', 'd-flex', 'justify-content-start', {active: currentStep >= 1 }]">
      <div class="step-circle-container">1</div>
      <span class="step-connect-line-1"></span>
      <div class="step-label-container">寄送地址</div>
    </div>
    <div 
    :class="['step', 'd-flex', 'justify-content-center', {active: currentStep >= 2 }]">
      <div class="step-circle-container">2</div>
      <span class="step-connect-line-2"></span>
      <div class="step-label-container">運送方式</div>
    </div>
    <div 
    :class="['step', 'd-flex', 'justify-content-end', {active: currentStep >= 3}]">
      <div class="step-circle-container">3</div>
      <div class="step-label-container">付款資訊</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Stepper',
  props:{
    currentStep:{
      type: Number,
      default: 1 // 預設為１
    }
  }
}
</script>



<style scoped lang="scss">
@import "../assets/main.scss";
.stepper-container {
  width: 100%;
  .step {
    display: flex;
    width: 100%;
    // border: 3px solid #000;
    position: relative;
    &-circle-container {
      width: 2rem;
      height: 2rem;
      margin: 1.5rem 0;
      text-align: center;
      line-height: 2rem;
      color: var(--nonactive-stepper);
      background-color: var(--body-bg);
      border: 1px solid var(--nonactive-stepper);
      border-radius: 50%;
    }
    &-connect-line-1 {
      display: block;
      height: 1px;
      background-color: var(--number);
      position: absolute;
      top: 2.5rem;
      left: 8rem;
      right: calc(-50% + 5rem);
    }
    &-connect-line-2 {
      display: block;
      height: 1px;
      background-color: var(--nonactive-stepper);
      position: absolute;
      top: 2.5rem;
      left: calc(50% + 5rem);
      right: calc(-100% + 8rem);
    }
    &-label-container {
      display: block;
      line-height: 2rem;
      margin: 1.5rem 0 1.5rem 0.5rem;
      color: var(--nonactive-stepper);
    }
  }
  .active {
    .step-circle-container {
      color: var(--cart);
      background-color: var(--active-stepper);
      border: 1px solid var(--active-stepper);
    }
    .step-label-container {
      color: var(--number);
    }
    span {
      background-color: var(--number);
    }
  }
}
</style>