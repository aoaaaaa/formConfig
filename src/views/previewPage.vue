<template>
    <div class="previewPage">
        <el-form label-positon='left' label-width='130px'>
            <el-row>
                <transition-group name='fade' tag='div' class="widget-form-list">
                    <template v-for='(element,index) in data.list'>
                        <el-col :span='element.colspan' :key='index'>
                            <el-form-item 
                                v-if='element'
                                class="widget-view"
                                :label='element.label'
                                :key='index'
                                :rules="!!element.isRule?((element.type=='input' || element.type=='textarea')?{required: true, message: '请输入', trigger: 'blur'}:{required: true, message: '请选择', trigger: 'change'}):{}">
                                <template v-if='element.type == "input"'>
                                    <el-input
                                        size='small'
                                        clearable
                                        v-model='element.defaultValue'
                                        :style='{width:element.width}'
                                        :placeholder='element.placeholder'
                                        :disabled='element.disabled'>
                                    </el-input>
                                </template>
                                <template v-if='element.type=="textarea"'>
                                    <el-input
                                        size='small'
                                        clearable
                                        type='textarea'
                                        :rows='element.rows'
                                        v-model='element.defaultValue'
                                        :style='{width:element.width}'
                                        :disabled='element.disabled'
                                        :placeholder='element.placeholder'>
                                    </el-input>
                                </template>
                                <template v-if='element.type=="time"'>
                                    <el-date-picker
                                        size='small'
                                        clearable
                                        :style="{width:element.width}"
                                        v-model='element.defaultValue'
                                        :placeholder='element.placeholder'
                                        :disabled='element.disabled'
                                        :format='element.format'
                                        :value-format='element.format'>
                                    </el-date-picker>
                                </template>
                                <template v-if='element.type=="date"'>
                                    <el-date-picker
                                        size='small'
                                        clearable
                                        v-model='element.defaultValue'
                                        :style="{width:element.width}"
                                        :placeholder='element.placeholder'
                                        :disabled='element.disabled'
                                        :format='element.format'
                                        :value-format='element.format'>
                                    </el-date-picker>
                                </template>
                                <template v-if='element.type=="rate"'>
                                    <el-rate
                                        v-model='element.defaultValue'
                                        :style='{width:element.width}'
                                        :texts='element.texts'
                                        :disabled='element.disabled'
                                        show-text>
                                    </el-rate>
                                </template>
                                <template v-if='element.type=="radio"'>
                                    <el-radio-group
                                        v-model='element.defaultValue'
                                        :style='{width:element.width}'
                                        :disabled='element.disabled'>
                                        <el-radio
                                            :label='item.value'
                                            v-for='(item,index) in element.options'
                                            :key='item.value + index'>
                                            {{item.label}}
                                        </el-radio>
                                    </el-radio-group>
                                </template>
                                <template v-if='element.type=="checkbox"'>
                                    <el-checkbox-group
                                        v-model='element.defaultValue'
                                        :style='{width:element.width}'
                                        :disabled='element.disabled'>
                                        <el-checkbox
                                            :label='item.value'
                                            v-for='(item,index) in element.options'
                                            :key='item.value + index'>
                                            {{item.label}}
                                        </el-checkbox>
                                    </el-checkbox-group>
                                </template>
                                <template v-if='element.type=="select"'>
                                    <el-select
                                        size='small'
                                        clearable
                                        v-model='element.defaultValue'
                                        :disabled='element.disabled'
                                        :style='{width:element.width}'
                                        :placeholder='element.placeholder'
                                        :filterable='element.remote'
                                        :remote='element.remote'
                                        :remote-method="remoteMethod">
                                        <el-option 
                                            v-for='item in element.options'
                                            :key='item.value'
                                            :value='item.value'
                                            :label='item.label'>
                                        </el-option>
                                    </el-select>
                                </template>
                                <template v-if='element.type=="title"'>
                                    <div class="el-titleInfo">
                                        {{element.defaultValue}}
                                    </div>
                                </template>
                            </el-form-item>
                        </el-col>
                    </template>
                </transition-group>
            </el-row>
        </el-form>
    </div>
</template>
<script>
    export default{
        name:'previewPage',
        props:['data'],
        data(){
            return{

            }
        },
        mounted(){
            console.log(this.data)
        },
        methods:{
            // 下拉框的搜索方法
            remoteMethod(val){

            }
        }
    }
</script>
<style>
    .previewPage{
        width: 100%;
        padding: 10px;
    }
</style>