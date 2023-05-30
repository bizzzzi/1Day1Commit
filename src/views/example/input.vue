<script setup>
const props = defineProps({
  useRegex: {
    type: Boolean,
    default: false,
  },
  useFormat: {
    type: Boolean,
    default: false,
  },
  regexRule: {
    type: Object,
  },
  modelValue: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['update:modelValue'])
const inputVal = computed({
  get() {
    if (props.useFormat && props.modelValue)
      return Intl.NumberFormat('ko-KR').format(props.modelValue)
    return props.modelValue
  },
  set(val) {
    const res = val.replace(props.regexRule, '')
    emit('update:modelValue', res)
  },
})

const inputRegex = (e) => {
  const res = props.regexRule.test(e.target.value)
  if (res && e.target.value)
    e.target.value = inputVal.value
}
</script>

<template>
  <div class="w-full flex justify-center">
    <div class="m-3 p-4 bg-gray-100 rounded-lg text-sm text-slate-700">
       <input
        v-if="useRegex"
        v-model="inputVal"
        type="text"
        class="border-1"
        @input="inputRegex"
      >
        <span>
          원하는 정규식 유형을 선택하여 텍스트를 입력하세요.
        </span>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
