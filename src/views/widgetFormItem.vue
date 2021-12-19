<template>
    <el-form-item 
        v-if='element && element.key'
        class="widget-view"
        :label='element.name'
        @click.native.stop='handleSelectWidget(index)'>
        <template v-if='element.type == "input"'>
            <el-input
                v-model='element.options.defaultValue'
                :style='{width:element.options.width}'
                :placeholder='element.options.placeholder'
                :disabled='element.options.disabled'>
            </el-input>
        </template>
        <template v-if='element.type=="textarea"'>
            <el-input
                type='textarea'
                :rows='element.options.rows'
                v-model='element.options.defaultValue'
                :style='{width:element.options.width}'
                :disabled='element.options.disabled'
                :placeholder='element.options.placeholder'>
            </el-input>
        </template>
        <template v-if='element.type=="time"'>
            <el-date-picker
                :style="{width:element.options.width}"
                v-model='element.options.defaultValue'
                :placeholder='element.options.placeholder'
                :disabled='element.options.disabled'
                :format='element.options.format'
                :value-format='element.options.format'>
            </el-date-picker>
        </template>
        <template v-if='element.type=="date"'>
            <el-date-picker
                v-model='element.options.defaultValue'
                :style="{width:element.options.width}"
                :placeholder='element.options.placeholder'
                :disabled='element.options.disabled'
                :format='element.options.format'
                :value-format='element.options.format'>
            </el-date-picker>
        </template>
        <template v-if='element.type=="rate"'>
            <el-rate
                v-model='element.options.defaultValue'
                :style='{width:element.options.width}'
                :texts='element.options.texts'
                :disabled='element.options.disabled'>
            </el-rate>
        </template>
        <template v-if='element.type=="radio"'>
            <el-radio-group
                v-model='element.options.defaultValue'
                :style='{width:element.options.width}'
                :disabled='element.options.disabled'>
                <el-radio
                    :label='item.value'
                    v-for='(item,index) in element.options.options'
                    :key='item.value + index'>
                    {{item.label}}
                </el-radio>
            </el-radio-group>
        </template>
        <template v-if='element.type=="checkbox"'>
            <el-checkbox-group
                v-model='element.options.defaultValue'
                :style='{width:element.options.width}'
                :disabled='element.options.disabled'>
                <el-checkbox
                    :label='item.value'
                    v-for='(item,index) in element.options.options'
                    :key='item.value + index'>
                    {{item.label}}
                </el-checkbox>
            </el-checkbox-group>
        </template>
        <template v-if='element.type=="select"'>
            <el-select
                v-model='element.options.defaultValue'
                :disabled='element.options.disabled'
                :style='{width:element.options.width}'
                :placeholder='element.options.placeholder'
                :filterable='element.options.remote'
                :remote='element.options.remote'
                :remote-method="remoteMethod">
                <el-option 
                    v-for='item in element.options.options'
                    :key='item.value'
                    :value='item.value'
                    :label='item.label'>
                </el-option>
            </el-select>
        </template>
        <template v-if='element.type=="transfer"'>
            <el-transfer
                filterable
                :filter-method="filterMethod"
                :filter-placeholder="element.options.placeholder"
                v-model="element.options.defaultValue"
                :data="element.options.options">
            </el-transfer>
        </template>
        <template v-if='element.type=="title"'>
            <div class="el-titleInfo">
                {{element.options.defaultValue}}
            </div>
        </template>
        <!-- <div class="widget-view-action" v-if="selectWidget.key == element.key">
          <i class="iconfont icon-shanchu" @click.stop="handleWidgetDelete(index)"></i>
        </div> -->
    </el-form-item>
</template>
<script>
    export default{
        name:'widgetFormItem',
        props:['element','select','index','data'],
        data(){
            return{
                selectWidget:this.select
            }
        },
        mounted(){
            console.log(this.element)
            console.log(this.select)
        },
        methods:{
            handleSelectWidget(index){
                this.selectWidget=this.data.list[index]
                console.log(this.selectWidget)
            },
            // 下拉框的搜索方法
            remoteMethod(val){
                console.log(val);
                console.log(this.element.options.remoteFunc);
            },
            // 穿梭框的搜索方法
            filterMethod(query,item){
                console.log(query);
                console.log(item);
            },
            // 删除被选中的组件
            handleWidgetDelete(index){

            }
        }
    }
</script>
<style>
    .widgetFormItem{

    }
    .widgetFormItem .active{
        box-shadow: 0 10px 15px 0 rgba(0,0,0,.1);
        border: 2px solid #2672ff;
        border-radius: 4px;
        outline: none;
        padding: 20px!important;
    }
</style>