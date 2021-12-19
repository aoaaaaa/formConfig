# formconfig

> 可视化表单配置的项目（自用）

## 页面展示



## 思路

> 表单配置页面分为三个模块，左侧工具组件，中间展示组件的效果，右侧对应组件的配置项
    因为是表单的配置页面，所以暂时在element-UI的基础之上，使用element的各种输入框、选择器、单选框等表单项。
    因为是可视化的配置，还需要使用拖拽组件，本身应该是拖拽到任何地方都可以定住，完全由使用者进行布局，但是那样便有些无法保证展示样式，于是这个项目只支持三种展示方式，即一行展示一个组件、两个组件、四个组件这三种模式，拖拽也只是用来改变组件的上下位置，而不是左右。

## Vuedraggable

> 这里使用的是vuedraggable。
    vuedraggable 是标准的组件式封装，并且将可拖动元素放进了transition-group上面，在使用的时候，可以通过v-model来双向绑定本地data，如果需要更新或者是触发父组件监听的事件，可以在updated()中去emit。
    具体使用方式没啥好说的，直接用就行
    下一个

## 

> 将页面分为三个模块，每个模块都是一个被封装好的组件，最终全放到一个父组件中作为页面展示。
> 监控拖拽组件的调用，被拖到中间展示区的组件存储到一个单独的list中，每个被选中的组件有着自己的可编辑的属性配置项，展示在右侧的框中，（通过最终的父组件传值到右侧的框中，其中判断当前组件的type来区分展示什么配置项）根据配置项的改变，来改变当前选中组件的属性的值。

## 数据格式

>commonComponents=[
    {
        type: 'input',
        stype:'',
        icon: 'icon-shurukuang',
        colspan:24,
        label:'单行文本',
        valueId:'',
        width: '100%',
        defaultValue: '',
        dataType: 'string',
        placeholder: '请输入',
        disabled: false,
        isRule:false
    },{
        type:'textarea',
        stype:'',
        icon:'icon-duohangwenben',
        colspan:24,
        label:'多行文本',
        valueId:"",
        width:'100%',
        defaultValue:'',
        placeholder:'请输入',
        rows:'2',
        dataType:'string',
        disabled:false,
        isRule:false
    },{
        type:'radio',
        stype:'',
        icon:'icon-danxuan',
        colspan:24,
        label:'单选框组',
        valueId:'',
        width:'100%',
        defaultValue:'',
        options: [
            {
                value: '0',
                label: '选项一'
            },{
                value: '1',
                label: '选项二'
            }
        ],
        interfaceFunc:'接口名',//这是定死的接口名
        remote:false,
        remoteFunc:'',
        disabled:false,
        isRule:false
    },{
        type:'select',
        stype:'',
        icon:'icon-duoxuan',
        colspan:24,
        label:"下拉选择",
        valueId:'',
        width:'100%',
        defaultValue:'',
        placeholder:'请选择',
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
        interfaceFunc:'接口名',//这是定死的接口名
        remote:false,
        remoteFunc:'',
        disabled:false,
        isRule:false
    },{
        type:'checkbox',
        stype:'',
        icon:'icon-duoxuan',
        colspan:24,
        label:'多选框组',
        valueId:'',
        width:'100%',
        defaultValue:[],
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
        interfaceFunc:'接口名',//这是定死的接口名
        remote:false,
        remoteFunc:'',
        disabled:false,
        isRule:false
    }
    ,{
        type:'upload',
        stype:'',
        icon:'icon-button-upload',
        colspan:24,
        label:'图片上传',
        defaultValue:'',
        action:'',
        disabled:false,
        autoUpload:false,
        listType:'picture',
        isRule:false
    },{
        type:'time',
        stype:'',
        icon:'icon-shijianbianjiwodeqianfenleishouye',
        colspan:24,
        label:"时间",
        valueId:'',
        width:'100%',
        defaultValue:'',
        placeholder:'请选择',
        format:'yyyy-MM-dd HH:mm:ss',
        disabled:false,
        isRule:false
    },{
        type:'date',
        stype:'',
        icon:'icon-riqi',
        colspan:24,
        label:'日期',
        valueId:'',
        width:'100%',
        defaultValue:'',
        placeholder:'请选择',
        format:'yyyy-MM-dd',
        disabled:false,
        isRule:false
    }
]

    type:类型  String   
        input:单行文本框
        textarea:多行文本框
        radio：单选框组
        checkbox：多选框组
        select:下拉选择框
        time：时间选择器
        date：日期选器
    icon:图标  String
    options:具体的配置项  Object
        width:宽度  String 百分比
        defaultValue:默认值  String
        dataType:值的类型  String
        placeholder:无值的时候展示的文字  String
        disabled:是否可以操作  Boolean
        step:每次改变时的最小单元改变数  Number
        options：radio、checkbox、select这些组件的选项的配置  Array
            value：值  String
            label：展示的文字  String
        remote:是否可以进行远程搜索  Boolean
        remoteFunc:远程搜索的方法名  String
        clearable：是否可以清空  Boolean
        arrowControl：是否使用箭头进行时间选择  Boolean
        format：时间格式  String