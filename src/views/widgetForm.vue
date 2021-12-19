<template>
    <div class="widgetForm">
        <el-form label-positon='left' label-width='130px'>
            <el-row>

                <Draggable
                    v-model='data.list'
                    @end='handleMoveEnd'
                    @add='handleWidgetAdd'>

                    <transition-group name='fade' tag='div' class="widget-form-list">
                        <template v-for='(element,index) in data.list'>
                            <el-col :span='element.colspan' :key='element.key'>
                                <el-form-item 
                                    v-if='element && element.key'
                                    class="widget-view"
                                    :class='selectWidget.key==element.key?"active":""'
                                    :label='element.label'
                                    @click.native.stop='handleSelectWidget(index)'
                                    :key='element.key'
                                    :rules="!!element.isRule?(element.type=='input'?{required: true, message: '请输入', trigger: 'blur'}:{required: true, message: '请输入', trigger: 'change'}):{}">
                                    <template v-if='element.type == "input"'>
                                        <el-input
                                            size='small'
                                            clearable
                                            v-model='element.defaultValue'
                                            :style='{width:element.width}'
                                            :placeholder='element.placeholder'
                                            :disabled='element.disabled'
                                            >
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
                                            type="datetime"
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
                                                :key='item.value'>
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
                                    <template v-if='element.type=="upload"'>
                                        <el-upload
                                            class="upload-demo"
                                            :action="element.action"
                                            :list-type="element.listType"
                                            :disabled='element.disabled'
                                            :auto-upload='element.autoUpload'>
                                            <el-button size="small" type="primary">点击上传</el-button>
                                            <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
                                        </el-upload>
                                    </template>
                                    <template v-if='element.type=="title"'>
                                        <div class="el-titleInfo">
                                            {{element.defaultValue}}
                                        </div>
                                    </template>
                                    <i class="iconfont icon-shanchu deleteicon" @click.stop="handleWidgetDelete(index)" v-if="selectWidget.key == element.key"></i>
                                </el-form-item>
                            </el-col>
                        </template>
                    </transition-group>

                </Draggable>
            </el-row>
            </el-form>
    </div>
</template>
<script>   
    import Draggable from 'vuedraggable'
    export default{
        name:'widgetForm',
        props:['data','select'],
        data(){
            return{
                selectWidget:this.select
            }
        },
        components:{
            Draggable,
        },
        watch:{
            data:{
                handler(val){
                    console.log(val)
                    // console.log(this.selectWidget)
                },
                deep:true
            },
            select:{
                handler(val){
                    console.log(val)
                    this.selectWidget=val;
                },
                deep:true
            },
            selectWidget:{
                handler(val){
                    // console.log(val)
                    this.$emit('chooseSelectWidget',val);
                },
                deep:true
            }
        },
        mounted(){
            // console.log(this.data)
        },
        methods:{
            // 拖动结束事件
            handleMoveEnd({newIndex,oldIndex}){
                console.log('index',newIndex,oldIndex)
            },
            // 添加组件事件
            handleWidgetAdd(evt){
                // const newIndex=this.data.list.length-1;
                // const key=Date.parse(new Date())+'_'+Math.ceil(Math.random()*99999)
                // console.log(key)
                // this.$set(this.data.list,newIndex,{
                //     ...this.data.list[newIndex],
                //     key
                // })
                // this.selectWidget=this.data.list[newIndex]
                // console.log(this.select) 
            },
            // 点击组件
            handleSelectWidget(index){
                // console.log(this.data.list)
                this.selectWidget=this.data.list[index]
            },
            // 下拉框的搜索方法
            remoteMethod(val){
                console.log(val);
                console.log(this.element.remoteFunc);
            },
            // 删除被选中的组件
            handleWidgetDelete(index){
                this.data.list.splice(index,1);
                this.$emit('changeSelectWidget',this.selectWidget);
            }
        }
    }
</script>
<style>
    .widgetForm{

    }
    .widgetForm .el-form-item{
        margin-bottom: 10px;
    }
    .widgetForm .el-form-item .el-titleInfo{
        margin-left: -80px;
        font-size: 16px;
        line-height: 32px;
        min-width: 80px;
        font-weight: 700;
        color: #303133;
    }
    .widgetForm .active{
        box-shadow: 0 10px 15px 0 rgba(0,0,0,.1);
        border: 2px solid #2672ff;
        border-radius: 4px;
        outline: none;
        padding: 20px!important;
        position: relative;
        cursor: move;
    }
    .widgetForm .deleteicon{
        text-align: center;
        right: -20px;
        width: 20px;
        position: absolute;
        bottom: -20px;
        font-size: 14px;
        color: #fff;
        cursor: pointer;
        height: 20px;
        line-height: 20px;
        background-color: #2672ff;
    }
    .widgetForm .el-rate{
        line-height: 40px;
        height: 40px;
    }
    .widgetForm .el-rate>.el-rate__item{
        vertical-align: sub;
    }
    .widgetForm .el-checkbox__inner{
        z-index: 0;
    }
</style>