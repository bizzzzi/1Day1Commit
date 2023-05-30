<script setup>
import CmInput from '../../views/example/input.vue'
const regexOptions = ref([
  {
    cdVal: 'REGEX_NUMBER',
    cdValNm: '숫자',
    cdValDesc: '0-9',
  },
  {
    cdVal: 'REGEX_KOREAN',
    cdValNm: '한글',
    cdValDesc: 'ㄱ-ㅎ가-힣',
  },
  {
    cdVal: 'REGEX_ENGLISH',
    cdValNm: '영문',
    cdValDesc: 'a-zA-Z',
  },
])

const inputSet = reactive({
  regexType: null,
  resultText: '',
  useYn: false,
  useFormat: false,
})

const handleUseRegex = (regex) => {
  inputSet.resultText = ''
  inputSet.useYn = true
  if (regex.cdVal === 'REGEX_NUMBER')
    inputSet.useFormat = true
  else inputSet.useFormat = false
  const inputRegexType = new RegExp(`[^${regex.cdValDesc}]`, 'g')
  inputSet.regexType = inputRegexType
}
</script>

<template>
  <div>
    <div>
      <el-button v-for="cd in regexOptions" :key="`regex-cd-${cd.cdVal}`" type="primary" class="border-1" @click="handleUseRegex(cd)">
        {{ cd.cdValNm }}
      </el-button>
    </div>
    <CmInput
      v-model="inputSet.resultText"
      :use-format="inputSet.useFormat"
      :use-regex="inputSet.useYn"
      :regex-rule="inputSet.regexType"
    />
  </div>
</template>

<style lang="scss" scoped></style>
