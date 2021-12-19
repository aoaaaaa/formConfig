<template>
    <div class="widgetConfig">
        <div class="widgetConfig-empty" v-if='!show'>请选择组件</div>
        <div v-if='show'>
            <!-- {{data.key}} -->
            <el-form label-position='left' label-width='68px'>
                <el-form-item label='标题'
                    v-if='data.type!=="title"'>
                    <el-input v-model='data.label' size='small'></el-input>
                </el-form-item>
                <el-form-item label='属性名'
                    v-if='data.type!=="title"'>
                    <el-input v-model='data.valueId' size='small'></el-input>
                </el-form-item>
                <el-form-item label='默认值'>
                    <el-input v-model='data.defaultValue' size='small' v-if='data.type!=="checkbox"'></el-input>
                    <el-input v-model='stringValue' size='small' v-if='data.type=="checkbox"'></el-input>
                </el-form-item>
                <el-form-item label='布局'
                    class="colspan">
                    <el-radio-group v-model="data.colspan">
                        <el-radio :label="24">一列</el-radio>
                        <el-radio :label="12">二列</el-radio>
                        <el-radio :label="6">四列</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label='是否禁用'
                    v-if='data.type!=="title"'>
                    <el-switch
                        v-model="data.disabled"
                        active-color="#13ce66"
                        inactive-color="#EBEEF5">
                    </el-switch>
                </el-form-item>
                <el-form-item label='校验'
                    v-if='data.type!=="title"'>
                    <el-switch
                        v-model="data.isRule"
                        active-color="#13ce66"
                        inactive-color="#EBEEF5">
                    </el-switch>
                </el-form-item>
                <!-- <el-form-item label='选项'
                    v-if='data.type=="select" || data.type=="radio" || data.type=="checkbox"'
                    class="select-options">
                    <el-radio-group 
                        v-model="data.remote" 
                        size="mini" 
                        style="margin-bottom:10px;">
                        <el-radio-button 
                            :label="false">
                            静态数据
                        </el-radio-button>
                        <el-radio-button 
                            :label="true"
                            v-if='data.type=="select"'>
                            远端数据
                        </el-radio-button>
                    </el-radio-group>
                    <template v-if='data.remote'>
                        <div class="interface-config">
                            <div class="interface-config-title">接口</div>
                            <el-select 
                                v-model="data.interfaceFunc" 
                                placeholder="请选择"
                                size='small'
                                v-if='data.stype==""'>
                                <el-option
                                    v-for='(item,index) in interfaceFuncs'
                                    :key='index'
                                    :value='item.url'
                                    :label='item.name'>
                                </el-option>
                            </el-select>
                            <el-select 
                                v-model="data.interfaceFunc" 
                                placeholder="请选择"
                                size='small'
                                v-if='data.stype!==""'>
                                <el-option
                                    v-for='(item,index) in [data.interfaceFunc]'
                                    :key='index'
                                    :value='item'
                                    :label='item'>
                                </el-option>
                            </el-select>
                        </div>
                    </template>
                    <template v-else>
                        <template v-if='data.type=="select" || data.type=="radio"'>
                            <el-radio-group
                                v-model='data.defaultValue'>
                                <Draggable 
                                    tag='ul' 
                                    :list='data.options'
                                    v-bind="{group:{ name:'options'}, ghostClass: 'ghost',handle: '.drag-item'}">
                                    <li 
                                        v-for='(item,index) in data.options' 
                                        :key='index'>
                                        <el-radio 
                                            :label='item.value' 
                                            style="margin-right: 5px">
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.value" 
                                                placement="top">
                                                <el-input 
                                                    size='mini' 
                                                    v-model='item.value'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.label" 
                                                placement="top">
                                                <el-input
                                                    size='mini'
                                                    v-model='item.label'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                        </el-radio>
                                        <i 
                                            class="drag-item" 
                                            style="font-size: 16px;margin: 0 5px;cursor: move;">
                                            <i class="iconfont icon-list"></i>
                                        </i>
                                        <el-button 
                                            @click='handleOptionsRemove(index)' 
                                            circle 
                                            plain 
                                            type="danger" 
                                            size="mini" 
                                            icon="el-icon-minus" 
                                            style="padding: 4px;margin-left: 5px;height: 22px;margin-top: 2px">
                                        </el-button>
                                    </li>
                                </Draggable>
                            </el-radio-group>
                        </template>
                        <template v-if='data.type=="checkbox"'>
                            <el-checkbox-group v-model='data.defaultValue'>
                                <Draggable
                                    tag='ul'
                                    :list='data.options'
                                    v-bind="{group:{ name:'options'}, ghostClass: 'ghost',handle: '.drag-item'}"
                                    handle='.drag-item'>
                                    <li 
                                        v-for='(item,index) in data.options'
                                        :key='index'>
                                        <el-checkbox
                                            :label='item.value'
                                            style="margin-right: 5px;">
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.value" 
                                                placement="top">
                                                <el-input 
                                                    size='mini' 
                                                    v-model='item.value'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.label" 
                                                placement="top">
                                                <el-input
                                                    size='mini'
                                                    v-model='item.label'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                        </el-checkbox>
                                        <i 
                                            class="drag-item" 
                                            style="font-size: 16px;margin: 0 5px;cursor: move;">
                                            <i class="iconfont icon-list"></i>
                                        </i>
                                        <el-button 
                                            @click='handleOptionsRemove(index)' 
                                            circle 
                                            plain 
                                            type="danger" 
                                            size="mini" 
                                            icon="el-icon-minus" 
                                            style="padding: 4px;margin-left: 5px;height: 22px;margin-top: 2px">
                                        </el-button>
                                    </li>
                                </Draggable>
                            </el-checkbox-group>
                        </template>
                        <div style='margin-left: 22px'>
                            <el-button 
                                type='text'
                                @click='handleAddOption'>
                                添加选项
                            </el-button>
                        </div>
                    </template>
                </el-form-item> -->
                <el-form-item label='选项'
                    v-if='data.type=="select" || data.type=="radio" || data.type=="checkbox"'
                    class="select-options">
                    <el-radio-group 
                        v-model="data.remote" 
                        size="mini" 
                        style="margin-bottom:10px;">
                        <el-radio-button 
                            :label="false">
                            静态数据
                        </el-radio-button>
                        <el-radio-button 
                            :label="true"
                            v-if='data.type=="select"'>
                            远端数据
                        </el-radio-button>
                    </el-radio-group>
                    <template v-if='data.remote'>
                        <div class="interface-config">
                            <div class="interface-config-title">接口</div>
                            <el-select 
                                v-model="data.interfaceFunc" 
                                placeholder="请选择"
                                size='small'
                                v-if='data.stype==""'>
                                <el-option
                                    v-for='(item,index) in interfaceFuncs'
                                    :key='index'
                                    :value='item.url'
                                    :label='item.name'>
                                </el-option>
                            </el-select>
                            <el-select 
                                v-model="data.interfaceFunc" 
                                placeholder="请选择"
                                size='small'
                                v-if='data.stype!==""'>
                                <el-option
                                    v-for='(item,index) in [data.interfaceFunc]'
                                    :key='index'
                                    :value='item'
                                    :label='item'>
                                </el-option>
                            </el-select>
                        </div>
                    </template>
                    <template v-else>
                        <template v-if='data.type=="select" || data.type=="radio"'>
                            <el-radio-group
                                v-model='data.defaultValue'>
                                <Draggable 
                                    tag='ul' 
                                    :list='options'
                                    v-bind="{group:{ name:'options'}, ghostClass: 'ghost',handle: '.drag-item'}">
                                    <li 
                                        v-for='(item,index) in options' 
                                        :key='index'>
                                        <el-radio 
                                            :label='item.value' 
                                            style="margin-right: 5px">
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.value" 
                                                placement="top">
                                                <el-input 
                                                    size='mini' 
                                                    v-model='item.value'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.label" 
                                                placement="top">
                                                <el-input
                                                    size='mini'
                                                    v-model='item.label'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                        </el-radio>
                                        <i 
                                            class="drag-item" 
                                            style="font-size: 16px;margin: 0 5px;cursor: move;">
                                            <i class="iconfont icon-list"></i>
                                        </i>
                                        <el-button 
                                            @click='handleOptionsRemove(index)' 
                                            circle 
                                            plain 
                                            type="danger" 
                                            size="mini" 
                                            icon="el-icon-minus" 
                                            style="padding: 4px;margin-left: 5px;height: 22px;margin-top: 2px">
                                        </el-button>
                                    </li>
                                </Draggable>
                            </el-radio-group>
                        </template>
                        <template v-if='data.type=="checkbox"'>
                            <el-checkbox-group v-model='data.defaultValue'>
                                <Draggable
                                    tag='ul'
                                    :list='options'
                                    v-bind="{group:{ name:'options'}, ghostClass: 'ghost',handle: '.drag-item'}"
                                    handle='.drag-item'>
                                    <li 
                                        v-for='(item,index) in options'
                                        :key='index'>
                                        <el-checkbox
                                            :label='item.value'
                                            style="margin-right: 5px;">
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.value" 
                                                placement="top">
                                                <el-input 
                                                    size='mini' 
                                                    v-model='item.value'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                            <el-tooltip 
                                                class="item" 
                                                effect="dark" 
                                                :content="item.label" 
                                                placement="top">
                                                <el-input
                                                    size='mini'
                                                    v-model='item.label'
                                                    style="width: 40%"
                                                    clearable>
                                                </el-input>
                                            </el-tooltip>
                                        </el-checkbox>
                                        <i 
                                            class="drag-item" 
                                            style="font-size: 16px;margin: 0 5px;cursor: move;">
                                            <i class="iconfont icon-list"></i>
                                        </i>
                                        <el-button 
                                            @click='handleOptionsRemove(index)' 
                                            circle 
                                            plain 
                                            type="danger" 
                                            size="mini" 
                                            icon="el-icon-minus" 
                                            style="padding: 4px;margin-left: 5px;height: 22px;margin-top: 2px">
                                        </el-button>
                                    </li>
                                </Draggable>
                            </el-checkbox-group>
                        </template>
                        <div style='margin-left: 22px'>
                            <el-button 
                                type='text'
                                @click='handleAddOption'>
                                添加选项
                            </el-button>
                        </div>
                    </template>
                </el-form-item>
                <el-form-item label='上传路径'
                    v-if='data.type=="upload"'>
                    <el-input v-model='data.action' size='small'></el-input>
                </el-form-item>
                <el-form-item label='展示类型'
                    v-if='data.type=="upload"'>
                    <el-select 
                        v-model="data.listType" 
                        placeholder="请选择"
                        size='small'>
                        <el-option
                            v-for='(item,index) in listTypes'
                            :key='index'
                            :value='item.value'
                            :label='item.label'>
                        </el-option>
                    </el-select>
                </el-form-item>

            </el-form>
        </div>
    </div>
</template>
<script>
    import Draggable from 'vuedraggable'
    export default{
        name:'widgetConfig',
        props:['data','interfaceFuncs','keyValue'],
        data(){
            return{
                listTypes:[
                    {
                        value:'text',
                        label:'文字'
                    },{
                        value:'picture',
                        label:'图片'
                    },{
                        value:"picture-card",
                        label:'图片卡片'
                    }
                ],
                options:[
                    {
                        value:'1',
                        label:'选项一'
                    },{
                        value:'2',
                        label:'选项二'
                    },{
                        value:'3',
                        label:'选项三'
                    }
                ],
                keyValues:[],
                dataOptions:[],
                stringValue:''
            }
        },
        components:{
            Draggable
        },
        computed:{
            show(){
                if(this.data && Object.keys(this.data).length>0){
                    return true
                }
                return false
            }
        },
        watch:{
            data:{
                handler:function(val){
                    var self=this;
                    this.stringValue=''
                    if(val.type=='checkbox'){
                        if(val.defaultValue instanceof Array&&val.defaultValue.length>0){
                            val.defaultValue.forEach((item,index)=>{
                                self.stringValue+=item+',';
                            })
                            self.stringValue=self.stringValue.slice(0,self.stringValue.length-1)
                        }
                    }
                },
                deep:true
            },
            keyValue:{
                handler:function(val){
                    var self=this;
                    if(this.keyValues.indexOf(val)!==-1){
                        this.dataOptions.find((item,index)=>{
                            if(item.keyValue==val){
                                self.options=item.options;
                            }
                        })
                        if(this.data.key=this.keyValue){
                            this.data.options=this.options;
                        }
                        return;
                    }
                    else{
                        this.keyValues.push(val);
                        console.log(this.queryList)
                        if(this.data.queryList){
                            this.options=this.data.options
                        }
                        else{
                            this.options=[
                                {
                                    value:'1',
                                    label:'选项一'
                                },{
                                    value:'2',
                                    label:'选项二'
                                },{
                                    value:'3',
                                    label:'选项三'
                                }
                            ];
                        }
                        // console.log(this.data.options);
                        // this.options=[
                        //     {
                        //         value:'1',
                        //         label:'选项一'
                        //     },{
                        //         value:'2',
                        //         label:'选项二'
                        //     },{
                        //         value:'3',
                        //         label:'选项三'
                        //     }
                        // ];
                        this.dataOptions.push({
                            keyValue:val,
                            options:this.options
                        })
                    }
                },
                deep:true
            },
            dataOptions:{
                handler:function(val){
                    var self=this;
                    val.find((item,index)=>{
                        if(item.keyValue==this.keyValue){
                            self.options=item.options;
                        }
                    })
                    if(this.data.key==this.keyValue){
                        this.data.options=this.options;
                    }
                },
                deep:true
            }
        },
        methods:{
            // 删除某选项
            handleOptionsRemove(index){
                var self=this;
                this.dataOptions.forEach((item,index2)=>{
                    if(item.keyValue==self.keyValue){
                        if(self.data.type=="checkbox"){
                            var valueArray=[];
                            this.stringValue='';
                            if(self.data.defaultValue.length>0){
                                self.data.defaultValue.forEach((item2,index2)=>{
                                    if(item2!==item.options[index].value){
                                        valueArray.push(item2)
                                        self.stringValue+=item2+','
                                    }
                                })
                                self.data.defaultValue=valueArray;
                                self.stringValue=self.stringValue.slice(0,self.stringValue.length-1);

                            }
                        }
                        else{
                            if(self.data.defaultValue==item.options[index].value){
                                self.data.defaultValue='';
                            }
                        }
                        // if(self.data.defaultValue==item.options[index].value){
                        //     self.data.defaultValue=''
                        // }
                        item.options.splice(index,1);
                    }
                })
            },
            // 添加选项
            handleAddOption(){
                const value=Math.random()
                this.dataOptions.forEach((item,index)=>{
                    if(item.keyValue==this.keyValue){
                        item.options.push({
                            value:'新选项'+value,
                            label:'新选项'
                        })
                    }
                })
            },
            
        }
    }
</script>
<style>
    .widgetConfig{
        height: 100%;
        position: relative;
    }
    .widgetConfig .widgetConfig-empty{
        height: fit-content;
        font-size: 30px;
        color: rgba(0, 0, 0, 0.2);
        margin: auto;
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        text-align: center;

    }
    .widgetConfig .el-form-item{
        margin-bottom: 10px;
    }
    .widgetConfig .select-options .el-form-item__content{
        margin-left: 0px !important;
    }
    .widgetConfig .select-options .el-form-item__content li{
        display: flex;
        margin-bottom: 5px;
    }
    .widgetConfig .select-options .el-form-item__content li .drag-item{
        line-height: 28px;
    }
    .widgetConfig .select-options .el-form-item__content .interface-config{
        display: flex;
    }
    .widgetConfig .select-options .el-form-item__content .interface-config .interface-config-title{
        width: 68px;
        margin-right: 10px;
        color: #606266;
    }
    .widgetConfig .colspan .el-radio-group .el-radio{
        margin-right: 20px;
    }
    .widgetConfig .el-checkbox__inner{
        z-index: 0;
    }
</style>