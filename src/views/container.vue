<!-- 表单配置页面 -->
<template>
    <div class="fc-container">
        <el-main class="fc-main">
            <el-header style="height: 40px;margin-bottom: 10px;">
                <el-button type='primary' size='small' @click='submit'>
                    <i class="iconfont icon-tijiao"></i>
                    <span>提交</span>
                </el-button>
                <el-button size='small' @click='preview' v-if='!pageShow'>
                    <i class="iconfont icon-yanjing1"></i>
                    <span>预览</span>
                </el-button>
                <el-button size='small' @click='closePreview' v-if='!!pageShow'>
                    <i class="iconfont icon-yanjing2"></i>
                    <span>收起预览</span>
                </el-button>
                <el-button size='small' @click='clearAll' v-if='!pageShow'>
                    <i class="iconfont icon-xiugai"></i>
                    <span>清空</span>
                </el-button>
            </el-header>
            <el-container class="contantView">
                <!-- 左侧组件栏 -->
                <el-aside width='240px' class="leftcon">
                    <template>
                        <div class="left-title">常用组件</div>
                        <ul>
                            <li 
                                v-for='(item,index) in basicComponents'
                                class="leftBtn"
                                :key='index'
                                @click='chooseComponents(item)'>
                                <i class="iconfont" :class='item.icon'></i>
                                <span>{{item.name}}</span>
                            </li>
                        </ul>
                    </template>
                    <template>
                        <div class="left-title">通用组件</div>
                        <ul>
                            <li 
                                v-for='(item,index) in commonComponents'
                                class="leftBtn"
                                :key='index'
                                @click='chooseComponents(item)'>
                                <i class="iconfont" :class='item.icon'></i>
                                <span>{{item.name}}</span>
                            </li>
                        </ul>
                    </template>
                    <template>
                        <div class="left-title">页面布局</div>
                        <el-radio-group v-model="allColSpan" @change='allColSpanChange'>
                            <el-radio :label="24">一列</el-radio>
                            <el-radio :label="12">二列</el-radio>
                            <el-radio :label="6">四列</el-radio>
                        </el-radio-group>
                    </template>
                </el-aside>
                <!-- 中间画布栏 -->
                <el-container class="center-container" direction='vertical'>
                    <el-main>
                        <div @click='clickMain' class="mainContainer">
                            <WidgetForm 
                                ref="widgetForm" 
                                :data='widgetFormData' 
                                :select.sync='widFormSelect'
                                @chooseSelectWidget='chooseSelectWidget'
                                v-if='widgetFormData.list.length>0'
                                @changeSelectWidget='changeSelectWidget'>
                            </WidgetForm>
                            <div class="form-empty" v-if='widgetFormData.list.length==0'>请选择左侧组件</div>

                        </div>
                    </el-main>
                </el-container>
                <!-- 右侧的配置栏 -->
                <el-aside class="config-content" width='250px'>
                    <WidgetConfig
                        :data='widFormSelect'
                        :interfaceFuncs='interfaceFuncs'
                        :keyValue='keyValue'>
                    </WidgetConfig>
                </el-aside>
            </el-container>
            <el-container v-if='pageShow' class="pageview">
                <PreviewPage 
                    :data='widgetFormData' >
                </PreviewPage>
            </el-container>
            <el-container class="dialogCon">
                <el-dialog title="提交表单配置" :visible.sync="submitVisible">
                    <el-form :model="submitform" :rules="submitRules" ref="submitRuleForm">
                        <el-form-item label="表单标识" prop="formKey">
                            <el-input v-model="submitform.formKey" style="width: 80%;" ></el-input>
                        </el-form-item>
                        <el-form-item label="表单名称" prop="formName">
                            <el-input v-model="submitform.formName" style="width: 80%;" ></el-input>
                        </el-form-item>
                    </el-form>
                    <div slot="footer" class="dialog-footer backbutton" style='textAlign:center'>
                        <el-button type="primary" @click="submitForm('submitRuleForm')" size='small'>提交</el-button>
                    </div>
                </el-dialog>
            </el-container>
        </el-main>
    </div>
</template>
<script>
    import Draggable from 'vuedraggable'
    import {basicComponents,commonComponents} from './componentsConfig.js'
    import WidgetForm from './widgetForm'
    import WidgetConfig from './widgetConfig'
    import PreviewPage from './previewPage'
    import axios from 'axios'
    export default{
        name:"fmConfigContainer",
        data(){
            return{
                basicComponents,
                commonComponents,
                basicFields:[
                    {
                        type:"select",
                        stype:'city',
                        name:'地市'
                    },{
                        type:'select',
                        stype:'county',
                        name:"区县"
                    },{
                        type:'select',
                        stype:'grid',
                        name:'网格'
                    },{
                        type:'input',
                        stype:'coPartnersPolicy',
                        name:'友商政策'
                    },{
                        type:'radio',
                        stype:'coPartners',
                        name:'友商组织'
                    },{
                        type:'radio',
                        stype:'requirementType',
                        name:'需求类型'
                    },{
                        type:"input",
                        stype:'oaNum',
                        name:"OA账号"
                    },{
                        type:'input',
                        stype:'title',
                        name:'工单标题'
                    },{
                        type:'textarea',
                        stype:'content',
                        name:'上报内容'
                    },{
                        type:'input',
                        stype:'place',
                        name:'详细地址'
                    }
                ],
                commomFields:[
                    {
                        type:"input",
                        stype:'',
                        name:'单行文本'
                    },{
                        type:"textarea",
                        stype:'',
                        name:'多行文本'
                    },{
                        type:"radio",
                        stype:'',
                        name:'单选框组'
                    },{
                        type:"select",
                        stype:'',
                        name:'下拉选择'
                    },{
                        type:"checkbox",
                        stype:'',
                        name:'多选框组'
                    },{
                        type:'upload',
                        stype:"",
                        name:'上传'
                    },{
                        type:'time',
                        stype:'',
                        name:'时间'
                    },{
                        type:'date',
                        stype:'',
                        name:'日期'
                    }
                ],
                widFormSelect: null,
                widgetFormData:{
                    formKey:'',
                    formName:"",
                    gridFlowItemForms:[],
                    list:[]
                },
                pageShow:false,
                allColSpan:24,
                submitVisible:false,
                submitform:{
                    formKey:'',
                    formName:""
                },
                submitRules:{
                    formKey: [
                        { required: true, message: '请输入Key', trigger: 'blur' }
                    ],
                    formName:[
                        { required: true, message: '请输入Key', trigger: 'blur' }
                    ]
                },
                interfaceFuncs:[],
                keyValue:"",
                queryList:false,
            }
        },
        components: {
            Draggable,
            WidgetForm,
            WidgetConfig,
            PreviewPage
        },
        mounted(){
            this._loadComponents();
            this.getInterface();
            console.log(this.$route.query);
            var query=this.$route.query;
            if(query.formList){
                this.queryList=true;
                this.widgetFormData.list=query.formList;
                this.widgetFormData.list.forEach((item,index)=>{
                    const key=Date.parse(new Date())+'_'+Math.ceil(Math.random()*99999);
                    item['key']=key;
                    item['queryList']=true;
                })
                console.log(this.widgetFormData)
                this.widFormSelect={};
            }
            else{
                this.queryList=false;
                this.setWidgetFormData();
            }
            // this.setWidgetFormData();
        },
        watch:{
            widgetFormData:{
                deep: true,
                handler: function (val) {
                    // console.log(this.$refs.widgetForm)
                }
            },
            widFormSelect:{
                handler:function(val){
                    // console.log(val)
                },
                deep:true
            }
        },
        methods:{
            setWidgetFormData(){
                this.widgetFormData.list=[
                    {
                        type:'input',
                        stype:'title',
                        icon:'icon-biaoti',
                        colspan:24,
                        label:'工单标题',
                        valueId:'taskName',
                        width:'100%',
                        defaultValue:'',
                        placeholder:'请输入',
                        dataType:'string',
                        disabled:false,
                        isRule:true
                    },{
                        type:'select',
                        stype:'city',
                        icon:'icon-dishi',
                        colspan:24,
                        label:"地市",
                        valueId:'regionCode',
                        width:'100%',
                        defaultValue:'',
                        placeholder:'请选择',
                        options:[
                            {
                                value:'1',
                                label:'默认'
                            }
                        ],
                        interfaceFunc:'地市接口',//这是定死的接口名
                        remote:false,
                        remoteFunc:'',
                        disabled:false,
                        isRule:true
                    },{
                        type:'select',
                        stype:'county',
                        icon:'icon-dishi',
                        colspan:24,
                        label:"区县",
                        valueId:'countyCode',
                        width:'100%',
                        defaultValue:'',
                        placeholder:'请选择',
                        options:[
                            {
                                value:'1',
                                label:'默认'
                            }
                        ],
                        interfaceFunc:'区县接口',//这是定死的接口名
                        remote:false,
                        remoteFunc:'',
                        disabled:false,
                        isRule:true
                    },{
                        type:'select',
                        stype:'grid',
                        icon:'icon-dishi',
                        colspan:24,
                        label:"网格",
                        valueId:'gridCode',
                        width:'100%',
                        defaultValue:'',
                        placeholder:'请选择',
                        options:[
                            {
                                value:'1',
                                label:'默认'
                            }
                        ],
                        interfaceFunc:'网格接口',//这是定死的接口名
                        remote:false,
                        remoteFunc:'',
                        disabled:false,
                        isRule:true
                    },{
                        type:'input',
                        stype:'place',
                        icon:'icon-dishi',
                        colspan:24,
                        label:'详细地址',
                        valueId:'detailedAddress',
                        width:'100%',
                        defaultValue:'',
                        placeholder:'请输入',
                        dataType:'string',
                        disabled:false,
                        isRule:true
                    },{
                        type:'textarea',
                        stype:'content',
                        icon:'icon-neirong',
                        colspan:24,
                        label:'上报内容',
                        valueId:"content",
                        width:'100%',
                        defaultValue:'',
                        placeholder:'请输入',
                        rows:'2',
                        dataType:'string',
                        disabled:false,
                        isRule:true
                    },{
                        type:'radio',
                        stype:'coPartners',
                        icon:'icon-zuzhi',
                        colspan:24,
                        label:'友商组织',
                        valueId:'coPartners',
                        width:'100%',
                        defaultValue:'',
                        options: [
                            {
                                value: '0',
                                label: '电信'
                            },{
                                value: '1',
                                label: '联通'
                            },{
                                value:'2',
                                label:'其他'
                            }
                        ],
                        disabled:false,
                        isRule:true
                    },{
                        type: 'input',
                        stype:'coPartnersPolicy',
                        icon: 'icon-zhengce',
                        colspan:24,
                        label:'友商政策',
                        valueId:'coPartnersPolicy',
                        width: '100%',
                        defaultValue: '',
                        dataType: 'string',
                        placeholder: '请输入',
                        disabled: false,
                        isRule:false
                    }
                ]
                this.widgetFormData.list.forEach((item,index)=>{
                    const key=Date.parse(new Date())+'_'+Math.ceil(Math.random()*99999);
                    item['key']=key;
                })
                console.log(this.widgetFormData)
                this.widFormSelect={};
            },
            _loadComponents(){
                this.basicComponents = this.basicComponents.map(item => {
                    var name='';
                    this.basicFields.forEach((item2,index2)=>{
                        if(item2.type==item.type&&item2.stype==item.stype){
                            name=item2.name;
                        }
                    })
                    return {
                        ...item,
                        name: name
                    }
                })
                this.commonComponents=this.commonComponents.map(item=>{
                    var name='';
                    this.commomFields.forEach((item2,index2)=>{
                        if(item2.type==item.type){
                            name=item2.name
                        }
                    })
                    return{
                        ...item,
                        name:name
                    }
                })
                
            },
            // 获取接口的数据
            getInterface(){
                this.interfaceFuncs=[];
                this.$fetch(this.$api.url.formConfig.getInterface)
                .then(res=>{
                    if(!!res&&res.status==0){
                        this.interfaceFuncs=res.obj;
                    }
                })
            },
            // 点击左侧按钮选择组件
            chooseComponents(item){
                var self=this;
                item.colspan=this.allColSpan;
                this.widgetFormData.list.push(item)
                const newIndex=this.widgetFormData.list.length-1;
                const key=Date.parse(new Date())+'_'+Math.ceil(Math.random()*99999)
                this.$set(this.widgetFormData.list,newIndex,{
                    ...this.widgetFormData.list[newIndex],
                    key
                })
                this.keyValue=key;
                this.widFormSelect=this.widgetFormData.list[this.widgetFormData.list.length-1];
                // this.$refs.widgetForm.handleWidgetAdd()
                event.stopPropagation()
            },
            handleMove(){
                return true
            },
            // 改变选择的组件
            chooseSelectWidget(val){
                this.widFormSelect=val;
                // console.log(val)
                this.keyValue=val.key;
                // console.log(this.keyValue)
            },
            // 预览按钮事件
            preview(){  
                // console.log(this.widgetFormData)
                this.pageShow=true;
            },
            // 收起预览事件
            closePreview(){
                this.pageShow=false;
            },
            clearAll(){
                this.widgetFormData.list=[];
                this.widFormSelect={};
            },
            // 点击中间空白区域，选中状态消失
            clickMain(){
                this.widFormSelect={};
            },
            // 左侧页面布局数据改变事件
            allColSpanChange(val){
                this.widgetFormData.list.forEach((item,index)=>{
                    item.colspan=val;
                })
            },
            // 中间的组件删除时，右侧配置项清空
            changeSelectWidget(val){
                this.widFormSelect={};
                this.keyValue=''
            },
            // 页面的提交按钮
            submit(){
                this.submitVisible=true;
            },
            // 弹出框的提交按钮
            submitForm(formName){
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        this.widgetFormData.formKey=this.submitform.formKey;
                        this.widgetFormData.formName=this.submitform.formName;
                        this.widgetFormData.gridFlowItemForms=this.widgetFormData.list;
                        console.log(this.widgetFormData)
                        axios.post(this.$api.url.formConfig.submitForm,this.widgetFormData)
                        .then(res=>{
                            if(!!res.data&&res.data.status==0){
                                this.submitVisible=false;
                                this.$router.push('/fmConfig/formList')
                            }
                            else{
                                this.$message({
                                    message:"提交失败"+res.data.msg,
                                    type:'error'
                                })
                            }
                            
                        })
                    }
                })
            }
        }
    }
</script>
<style>
    .fc-container{
        height: 100%;
        padding: 10px;
        background-color: rgba(0,0,0,0.04);
    }
    .fc-container>.fc-main{
        margin: 0;
        padding: 0;
        width: 100%;
        height: 100%;
        position: relative;
    }
    .fc-container>.fc-main>.contantView{
        height: calc(100% - 50px);
        background-color: #fff;
        /* border: 1px solid #ccc; */
    }
    .fc-container>.fc-main .leftcon{
        border-right: 2px dashed #ccc; 
        /* position: absolute; */
        height: 100%;
        padding: 0 10px;
        height: calc(100% - 20px);
        margin: 10px 0;
    }
    .fc-container>.fc-main .leftcon::-webkit-scrollbar{
        display: none;
    }
    .fc-container>.fc-main .leftcon>.left-title{
        margin-bottom: 10px;
        margin-top: 20px;
    }
    
    .fc-container>.fc-main .leftcon ul{
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
    }
    .fc-container>.fc-main .leftcon ul>li{
        width: 48%;
        font-size: 12px;
        line-height: 30px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        margin: 1%;
        color: #333;
        border: 1px solid #d8d8d8;
        padding-left: 10px;
    }
    .fc-container>.fc-main .leftcon ul>li:hover{
        border-color: #2672ff;
        color: #2672ff;
    }
    .fc-container>.fc-main .leftBtn{
        cursor: pointer;
        border: 1px solid #66B1ff;
    }
    .fc-container>.fc-main .leftBtn>i{
        margin-right: 10px;
    }
    .fc-container>.fc-main .config-content{
        border-left: 2px dashed #ccc;
        height: calc(100% - 20px);
        padding: 0 10px;
        margin: 10px 0;
    }
    .fc-container .el-main{
        width: 100%;
    }
    .fc-container .el-main .mainContainer{
        height: 100%;
        position: relative;
    }
    .fc-container .config-content{
        padding: 10px;
    }
    .fc-container .config-content::-webkit-scrollbar{
        display: none;
    }
    .fc-container .center-container>.el-main::-webkit-scrollbar{
        display: none;
    }
    .fc-container>.fc-main>.pageview{
        background-color: #fff;
        overflow-y: scroll;
        height: calc(100% - 50px);
        position: absolute;
        top: 50px;
        width: 100%;
    }
    .fc-container>.fc-main>.pageview::-webkit-scrollbar{
        display: none;
    }
    .fc-container .mainContainer .form-empty{
        font-size: 40px;
        color: rgba(0,0,0,0.2);
        text-align: center;
        margin: auto;
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        height: fit-content;
    }
</style>