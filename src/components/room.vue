<template>
  <div class="header">
    <div>
      <el-breadcrumb separator="/" :separator-icon="ArrowRight">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>批量预定</el-breadcrumb-item>
      </el-breadcrumb>
    </div>

    <div class="main">
      <el-card>
        <el-form
          ref="ruleFormRef"
          style="max-width: 600px"
          :model="ruleForm"
          :rules="rules"
          label-width="auto"
          class="demo-ruleForm"
          status-icon
        >
          <el-form-item label="选择疗养所" prop="name">
            <el-cascader placeholder="请输入所点名称" :options="options" filterable />
          </el-form-item>
        </el-form>
      </el-card>
    </div>

    <div class="footer">
      <el-card>
        <el-table :data="tableData" style="width: 100%" max-height="1000">
          <el-table-column fixed prop="checkTime" label="入住日期" width="174">
            <template #default="scope">
              <el-date-picker
                v-model="scope.row.checkTime"
                type="date"
                placeholder="请选择"
                :size="size"
              />
            </template>
          </el-table-column>
          <el-table-column prop="leavetime" label="离店日期" width="174">
            <template #default="scope">
              <el-date-picker
                v-model="scope.row.leavetime"
                type="date"
                placeholder="请选择"
                :size="size"
              />
            </template>
          </el-table-column>
          <el-table-column prop="room" label="选择客房" width="174">
            <template #default="scope">
              <el-select
                v-model="scope.row.room"
                placeholder="请选择"
                size="default"
                style="width: 140px"
              >
                <el-option
                  v-for="item in optionsRoom"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                />
              </el-select>
            </template>
          </el-table-column>
          <el-table-column prop="number" label="预订数量" width="174">
            <template #default="scope">
              <el-input-number
                v-model="scope.row.number"
                :min="1"
                :max="10"
                @change="handleChange"
              />
            </template>
          </el-table-column>
          <el-table-column prop="people" label="选择预订人" width="174">
            <template #default="scope">
              <el-select
                v-model="scope.row.people"
                placeholder="请选择"
                size="default"
                style="width: 140px"
              >
                <el-option
                  v-for="item in optionsPeople"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                />
              </el-select>
            </template>
          </el-table-column>
          <el-table-column prop="limitBefore" label="该预订人订房前额度" width="174">
            {{ limitBefore }}
          </el-table-column>
          <el-table-column prop="limitAfter" label="使用额度" width="174">
            {{ limitAfter }}
          </el-table-column>
          <el-table-column fixed="right" label="操作" width="174">
            <template #default="scope">
              <el-button link type="primary" size="small" @click.prevent="deleteRow(scope.$index)">
                删除
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-card>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'Room',
}
</script>

<script setup lang="ts">
import { ArrowRight } from '@element-plus/icons-vue'

import { reactive, ref } from 'vue'
import type { FormRules } from 'element-plus'

const ruleForm = reactive({
  name: 'Hello',
})

const rules = reactive<FormRules>({
  name: [
    { required: true, message: '请选择疗养所', trigger: 'blur' },
    { min: 3, max: 5, message: 'Length should be 1 to 10', trigger: 'blur' },
  ],
})

const options = [
  {
    value: '哈尔滨疗养所',
    label: '哈尔滨疗养所',
  },
  {
    value: '北京疗养所',
    label: '北京疗养所',
  },
]

const tableData = ref([
  {
    checkTime: '',
    leavetime: '',
    room: '',
    number: ' ',
    people: '',
    limitBefore: '',
    limitAfter: '',
  },
])

const size = ref<'default' | 'large' | 'small'>('default')

const optionsRoom = [
  {
    value: 'Option1',
    label: '标准间',
  },
  {
    value: 'Option1111',
    label: ' 大床房',
  },
]

const handleChange = (value: number) => {
  console.log(value)
}

const optionsPeople = [
  {
    value: 'Option1',
    label: '张三',
  },
  {
    value: 'Option1111',
    label: '李四',
  },
]

const deleteRow = (index: number) => {
  tableData.value.splice(index, 1)
}

const limitBefore = ref<string>('0晚')
const limitAfter = ref<string>('0晚')

const onAddItem = () => {
  const newRow = {
    checkTime: '',
    leavetime: '',
    room: '',
    number: ' ',
    people: '',
    limitBefore: '',
    limitAfter: '',
  }
  tableData.value.push(newRow)
}
</script>

<style scoped>
.header {
  margin: 50px;
}

.main,
.footer {
  margin-top: 30px;
}

:deep .el-date-editor {
  --el-date-editor-width: 140px;
  padding: 0;
}

:deep .el-table .cell {
  text-align: center;
}
</style>
