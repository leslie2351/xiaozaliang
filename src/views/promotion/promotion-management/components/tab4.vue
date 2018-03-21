<!-- 已上架页面 -->
<style lang="less">

    @import '../../../../styles/common.less';
    @import '../../../../styles/loading.less';
        .promotion-t{
            width: 100px;
            text-align:right;
            line-height: 32px;
            
        }
        .cheackMain{
            border:1px solid #ccc;
            display:inline-block;
            width: 250px;
            height: 90px;
            margin: 5px 10px ;
            padding: 5px;
        }
        .cheackBox{
            
            img{
                margin-left: 5px;
                width: 80px;
                height: 75px;
            }
            div{
                width: 130px;
                font:12px/24px '微软雅黑';
            }
        }
         .w-e-text-container{
            height: 200px !important;
        }
        .activity-information{
            position: relative;
            .ivu-select-dropdown{
                top:35px !important;
            }
        }
        .preferential-way{
            .ivu-select-dropdown{
                top:32px !important;
            }
        }
        .shen{
            overflow:hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
        }
        
</style>

<template>
    <div>
        <Row>
            <Col span="24">
                <div class='clearfix'>

                    <div class="margin-bottom-10 clearfix">
                        <Row class='fl margin-right-10'>
                            <Input v-model="searchConName" @on-click="initTable" icon="search" placeholder="请输入活动名称搜索..." style="width: 200px" />
                        </Row>
                        <Button type="primary" class="fr" @click="btnAdd">新增</Button>
                    </div>
                    <Row class="margin-top-10 searchable-table-con1" justify="center" align="middle">
                        <Table border :columns="orderColumns" :data="orderData"></Table>
                    </Row>
                    <div class="clearfix">

                        <Page :total=total size="small" class='fr margin-top-20' show-elevator show-sizer show-total
                            @on-change = 'changePage' @on-page-size-change='changePageSize' placement='top'></Page>
                    </div>
                    <Modal
                        v-model="modal3"
                        :title= status
                        width='70%;'
                        :styles="{top: '20px'}"
                        @on-ok="okAdd"
                        @on-cancel="cancelAdd">
                        <div>
                            <div class="clearfix">
                                <!-- <div class="fl" style="width:50%;"> -->
                                <div class="">
                                    <h4 class="padding-left-20">活动信息</h4>
                                    <div class="padding-top-10">
                                        <span class="fl promotion-t margin-right-10">活动名称：</span>
                                        <Input v-model="value1" placeholder="..." style="width: 250px"></Input>
                    
                                    </div>
                                    <div class="padding-top-10 clearfix">
                                        <span class="fl promotion-t margin-right-10">活动内容：</span>
                                        <div class="fl">
                                            <div>
                                                <div ref="editor" style="text-align:left;" class="text"></div>
                                                <button v-on:click="getContent">查看内容</button>
                                               
                                            </div>
                                        </div>
                                    </div>
                                    <div class="activity-information">
                                        <div class="padding-top-10">
                                            <span class="fl promotion-t margin-right-10">生效时间：</span>
                                            <DatePicker type="datetime" format='yyyy-MM-dd HH:mm:ss' :value="value2" placement='bottom' placeholder="生效时间" @on-change='changeDateT' style="width: 200px;"></DatePicker>
                        
                                        </div>
                                        <div class="padding-top-10">
                                            <span class="fl promotion-t margin-right-10">结束时间：</span>
                                            <DatePicker type="datetime" format='yyyy-MM-dd HH:mm:ss' :value="value3" placement='bottom' placeholder="结束时间" @on-change='changeDateS' style="width: 200px"></DatePicker>
                        
                                        </div>
                                    </div>
                                </div>
                                <!-- <div class="fl" style="width:50%;"> -->
                                <div style="padding-top:20px;">
                                    <h4 class="padding-left-20">优惠设置</h4>
                                    <div class="padding-top-10 clearfix">
                                        <span class="fl promotion-t margin-right-10">满件数：</span>
                                        <div style="width: 250px" class="fl">
                                            <Input v-model="value4" placeholder="..." style="width: 100px"></Input> 件
                    
                                        </div>
                                    </div>
                                    <div class="padding-top-10 clearfix">
                                        <span class="fl promotion-t margin-right-10">免件数：</span>
                                        <div style="width: 250px" class="fl">
                                            <Input v-model="value5" placeholder="..." style="width: 100px"></Input> 件
                    
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div style="" class="clearfix">
                                <h4 class="padding-left-20 padding-top-20 fl margin-right-10">参与活动商品：</h4>
                                <RadioGroup v-model="disabledGroup" class="fl" style="padding-top:20px;" @on-change='btnD'>
                                    <Radio label="全部商品参与"></Radio>
                                    <Radio label="部分商品参与"></Radio>
                                    
                                </RadioGroup>
                            </div>

                            <div style="margin-left:112px;border:1px solid #ccc;" class="margin-top-10" v-if='flag'>
                                <div class="clearfix" style="padding-left:16px;">
                                    <div class="padding-top-10 clearfix fl">
                                        <span class="fl margin-right-10" style="line-height: 32px;">筛选：</span>
                                        
                                        <Cascader :data="data" class='fl margin-right-10' @on-change='btnSee' change-on-select></Cascader>
                                    </div>
                                    <Row class='fl margin-top-10'>
                                        <Input v-model="searchConName2" @on-click="getcommdityList" icon="search" placeholder="请输入搜索..." style="width: 200px" />
                                    </Row>
                                </div>
                                <Tabs value="name1" size="small" :animated="false">
                                    <TabPane label="选择商品" name="name1">
                                        <CheckboxGroup v-model="checkAllGroup" @on-change="checkAllGroupChange" style="max-height:200px; overflow-y: scroll;">
                                            <div class="cheackMain" v-for="item in comndityLit">
                                               <Checkbox :label=item.id>
                                                    <div class="fr cheackBox">
                                                        <img src="" alt="" class="fl">
                                                        <div class="fl">
                                                            <div style="padding-left:3px;" class="shen">名称：{{item.goodsName}}</div>
                                                            <div style="padding-left:3px; padding-top:0px;">
                                                                价格：{{item.salePrice}}
                                                                
                                                            </div>
                                                            <div style="padding-left:3px; padding-top:0px;">
                                                                库存：{{item.availableStock}}
                                                            </div>
                                                        </div>
                                                    </div>
                                                </Checkbox> 
                                                
                                            </div>
                                            
                                        </CheckboxGroup>
                                        <div style="margin-left:10px;border-top:1px solid #ccc" class="padding-top-10 padding-bottom-10">
                                            <Checkbox
                                                :indeterminate="indeterminate"
                                                :value="checkAll"
                                                @click.prevent.native="handleCheckAll">全选
                                            </Checkbox>
                                            <Button type="primary" size="small" @click = 'btnAddC'>添加</Button>
                                        </div>
                                        
                                    </TabPane>
                                    <TabPane label="已选商品" name="name2">
                                        <CheckboxGroup v-model="checkAllGroupT" @on-change="checkAllGroupChangeT" style="max-height:200px; overflow-y: scroll;">
                                            <div class="cheackMain" v-for="item,index in comndityLitT">
                                               <Checkbox :label=item.id>
                                                    <div class="fr cheackBox">
                                                        <img src="" alt="" class="fl">
                                                        <div class="fl">
                                                            <div style="padding-left:3px;" class="shen">名称：{{item.goodsName}}</div>
                                                            <div style="padding-left:3px; padding-top:0px;">
                                                                价格：{{item.salePrice}}
                                                                
                                                            </div>
                                                            <div style="padding-left:3px; padding-top:0px;">
                                                                库存：{{item.availableStock}}
                                                            </div>
                                                        </div>
                                                    </div>
                                                </Checkbox> 
                                                
                                            </div>
                                            
                                        </CheckboxGroup>
                                        <div style="margin-left:10px;border-top:1px solid #ccc" class="padding-top-10 padding-bottom-10">
                                            <Checkbox
                                                :indeterminate="indeterminateT"
                                                :value="checkAllT"
                                                @click.prevent.native="handleCheckAllT">全选
                                            </Checkbox>
                                            <Button type="primary" size="small" @click = 'DelAddC'>移除</Button>
                                        </div>
                                    </TabPane>
                                    
                                </Tabs>
                            </div>
                        </div>
                    </Modal>
                    <Modal
                        v-model="modal1"
                        title="确认删除"
                        @on-ok="okDel"
                        @on-cancel="cancelDel">
                        <p>确定删除吗？删除后无法恢复</p>

                    </Modal>

                </div>
            </Col>
        </Row>
    </div>
</template>

<script>

import E from 'wangeditor'
export default {
    name: 'text-editor',
    props:['start'],
    data () {
        return {
            editorContent: '',
            orderColumns: [
                {
                    title: '活动名称',
                    key: 'activity_name',
                    align: 'center'
                },
                {
                    title: '开始时间',
                    key: 'start_date',
                    align: 'center'
                },
                {
                    title: '结束时间',
                    key: 'end_date',
                    align: 'center'
                },
                {
                    title: '满件数',
                    key: 'full_goods_num',
                    align: 'center'
                },
                {
                    title: '免件数',
                    key: 'reduce_goods_num',
                    align: 'center'
                },
                {
                    title: '操作',
                    key: 'operation',
                    align: 'center',
                    render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'primary',
                                        size: 'small'
                                    },
                                    on: {
                                        click: () => {
                                            this.btnAttr(params.index)
                                         
                                        }
                                    }
                                }, '修改'),
                                h('Button', {
                                    props: {
                                        type: 'error',
                                        size: 'small'
                                    },
                                    style: {
                                        marginLeft:"10px"
                                    },
                                    on: {
                                        click: () => {
                                            this.del(params.index)
                                        }
                                    }
                                }, '删除'),                              
                            ])
                    }
                }

            ],
            orderData: [],
            searchConName:'',

            id:'',//
            status:"新增",
            //点击新增时
            modal3:false,
            //删除
            modal1:false,
            //编辑
            value1:'',
            value2:'', //生效时间
            value3:'',
            value4:'',
            value5:'',
            editorHtml:'',


            disabledGroup:'全部商品参与',
            name1:'name1',
            flag:false,
            
            create_date:'',//创建时间
            //分组选择
            groupId:'',
            data: [], //筛选分组框
            searchConName2:'',
            
            //多选框配置
            //全部
            comndityLit:[],
            indeterminate: true,
            checkAll: false,
            checkAllGroup: [], //已选择的商品
            checkAllGroupId:[],//全部商品id

            // 已选
            comndityLitT:[],
            indeterminateT: true,
            checkAllT: false,
            checkAllGroupT: [], //已选择的商品
            checkAllGroupIdT:[],//全部商品id

            ids:[],//最终要发送的已选商品
            total:0,//总页数
            current:1,//当前页码
            pageSize:10,//每页数量

            //富文本编辑器
            editor:{},
            
        };
    },

    methods:{
        getContent: function () {
            alert(this.editorContent)
            
        },
        
        changePage(val){ //切换页码
            this.current = val
            this.initTable()
        },
        changePageSize(val){ //改变每页显示条数
            this.pageSize = val
            this.initTable()

        },
        btnAdd(){ //点击新增时
            this.modal3 = true;
            this.status = '新增';
            //格式化输入框
            this.value1 = '';
            this.editor.txt.clear()
            this.value2 = ''
            this.value3 = ''
            this.value4 =''
            this.value5 = ''
            this.disabledGroup='全部商品参与';
            this.flag = false;
            this.searchConName2 =''
            this.groupId = ''
            this.checkAllGroup = []
            this.getcommdityList() // 获取所有商品数
            //已选商品
            this.comndityLitT=[];
            this.checkAllGroupIdT =[]
        },


        //获取所有商品
        getcommdityList(){
            this.checkAllGroupId = []
            this.$http.post('http://192.168.5.11:8080/storeGoods/list',{
                storeId:6666,
                status:1,
                goodsName:this.searchConName2,
                groupId:this.groupId,//分组id
            }).then((data)=>{
                this.comndityLit = data.data
                data.data.forEach((item)=>{
                    this.checkAllGroupId.push(item.id)
                })
                
            }).catch((err)=>{
                console.log(err);
                
            })
        },
        initSeero(){ //获取下拉框内容
            this.$http.post('http://192.168.5.11:8080/storeGroup/storeGroupShow',{
                storeId:'6666'
            }).then((data)=>{
                this.AtterSee(data.data)
                
            }).catch((err)=>{
                console.log(err);
                
            })
        },
        //过滤筛选，调整筛选内容为前段需要的
        AtterSee(data){
            data.forEach((item)=>{
                item.value = item.id;
                item.label = item.groupName;
                if(item.storeGroupInfoPOs !=undefined){
                    item.children = item.storeGroupInfoPOs;
                    this.AtterSee(item.children)
                }

            })
            this.data = data;      
        },
        btnSee(val){ //点击分组选择器
            console.log(val);
            this.groupId = val[val.length-1]
            console.log(this.groupId);
            this.getcommdityList()

        },
        btnD(val){ //切换/全部商品参与/部分商品参与
            console.log(val);
            if(val == '全部商品参与'){
                this.flag = false;
            }else if(val == '部分商品参与'){
                this.flag = true;
            }
        },

        //全部商品
        //点击全选
        handleCheckAll () {
            if (this.indeterminate) {
                this.checkAll = false;
            } else {
                this.checkAll = !this.checkAll;
            }
            this.indeterminate = false;

            if (this.checkAll) {
                this.checkAllGroup = this.checkAllGroupId;
            } else {
                this.checkAllGroup = [];
            }
        },
        checkAllGroupChange (data) {
            console.log('选中的');
            
            console.log(data);
            this.checkAllGroup = data;
            if (data.length ===  this.checkAllGroupId.length) {
                this.indeterminate = false;
                this.checkAll = true;
            } else if (data.length > 0) {
                this.indeterminate = true;
                this.checkAll = false;
            } else {
                this.indeterminate = false;
                this.checkAll = false;
            }
        },
        // 点击商品添加时
        btnAddC(){
            this.checkAllGroupT =[]
            this.checkAllGroupIdT =[]
            this.ids = []
            this.comndityLitT = this.comndityLit.filter((item,index)=>{
                return this.checkAllGroup.indexOf(item.id) != -1;
            })
            this.comndityLitT.forEach((item)=>{
                this.checkAllGroupIdT.push(item.id)
                this.ids.push(item.id)
            })
        },
        //已选商品
        handleCheckAllT () {
            if (this.indeterminateT) {
                this.checkAllT = false;
            } else {
                this.checkAllT = !this.checkAllT;
            }
            this.indeterminateT = false;

            if (this.checkAllT) {
                this.checkAllGroupT = this.checkAllGroupIdT;
            } else {
                this.checkAllGroupT = [];
            }
        },
        checkAllGroupChangeT (data) {
            console.log('选中的');
            
            console.log(data);
            console.log(this.checkAllGroupIdT);
            
            this.checkAllGroupT = data;
            if (data.length ===  this.checkAllGroupIdT.length) {
                this.indeterminateT = false;
                this.checkAllT = true;
            } else if (data.length > 0) {
                this.indeterminateT = true;
                this.checkAllT = false;
            } else {
                this.indeterminateT = false;
                this.checkAllT = false;
            }
        },
        //已选商品移除
        DelAddC(){
            this.checkAllGroup =[]
            this.ids =[]
            let abc = this.comndityLitT.filter((item,index)=>{
                return this.checkAllGroupT.indexOf(item.id) == -1;
            })
            console.log(abc);
            this.comndityLitT = abc;
            this.checkAllGroupT =[];
            this.comndityLitT.forEach((item)=>{
                this.checkAllGroup.push(item.id)
                this.ids.push(item.id)
            })

        },
        //新增修改
        changeDateT(val){

            this.value2 = val
        },
        changeDateS(val){

            this.value3 = val
        },
        okAdd(){ //确认添加

            let is_all = this.disabledGroup == "全部商品参与"  ? 1 : 0;

            var str ='' //选中的商品id转换为字符串类型
            if(is_all == 0){
                
                this.ids.forEach((item)=>{
                    str += item + ","
                })

                str = str.substr(0,str.length-1);
            }else{
                str =''
            }
            
            if(this.status == '新增'){
                this.$http.post('http://192.168.5.21:8080/store-promotion-info/add',{
                    store_id:6666,
                    activity_name:this.value1,
                    start_date:this.value2,
                    end_date:this.value3,
                    activity_description:this.editorContent,
                    full_goods_num:this.value4,
                    reduce_goods_num:this.value5,
                    is_all : is_all,
                    ids:str,
                    promotion_type:4,
                }).then((data)=>{
                    if(data.data.result == 1){
                        this.$Message.info(data.data.message);
                        this.initTable()
                    }else{
                        alert(data.data.message)
                    }
                }).catch((err)=>{
                    console.log(err);
                    
                })
            }else if(this.status == '修改'){

                if(this.editorContent == ''){
                    this.editorContent=this.editorHtml;
                }
                this.$http.put('http://192.168.5.21:8080/store-promotion-info/edit',{
                    store_id:6666,
                    id:this.id,
                    create_date:this.create_date,
                    activity_name:this.value1,
                    start_date:this.value2,
                    end_date:this.value3,
                    activity_description:this.editorContent,
                    full_goods_num:this.value4,
                    reduce_goods_num:this.value5,
                    is_all : is_all,
                    ids:str,
                    promotion_type:4,
      
                }).then((data)=>{
                    if(data.data.result == 1){
                        this.$Message.info(data.data.message);
                        this.initTable()
                    }else{
                        alert(data.data.message)
                    }
                }).catch((err)=>{
                    console.log(err);
                    
                })
            }
        },
        cancelAdd(){
            this.$Message.info('取消添加');
        },
        //修改
        btnAttr(index){
            this.modal3  = true;
            this.status = "修改"
            this.id = this.orderData[index].id;
            this.ids = []
            this.create_date = this.orderData[index].create_date;
            this.value1 = this.orderData[index].activity_name;
            this.editor.txt.html(this.orderData[index].activity_description)
            this.editorHtml=this.orderData[index].activity_description;
            this.value2 = this.orderData[index].start_date;
            this.value3 = this.orderData[index].end_date;

            this.value4 = this.orderData[index].full_goods_num;
            this.value5 = this.orderData[index].reduce_goods_num

            //查看是否全部商品参与
            this.getcommdityList() // // 获取所有商品
            this.checkAllGroup = []; // 先清空已选商品

            if(this.orderData[index].is_all == 0){
                this.disabledGroup = '部分商品参与'
                this.flag = true;
                this.getPromotionGoods() //参与的商品信息
            }else{
                this.disabledGroup = '全部商品参与'
                this.flag = false;
            }
            // this.getPromotionGoods()
        },
        //获取当前活动id 参与的商品信息
        getPromotionGoods(){   
            this.$http.get('http://192.168.5.21:8080/store-promotion-info/getPromotionGoods/'+this.id)
            .then((data)=>{
                this.checkAllGroup=[]

                data.data.forEach((item)=>{
                    this.checkAllGroup.push(item.goods_id)
                })

                this.comndityLitT = this.comndityLit.filter((item,index)=>{
                    return this.checkAllGroup.indexOf(item.id) != -1;
                })
                this.comndityLitT.forEach((item)=>{
                    this.checkAllGroupIdT.push(item.id)
                    this.ids.push(item.id)
                })
            }).catch((err)=>{
                console.log(err)

            })
        },

        del(val){  //删除
            this.modal1=true;
            this.id = this.orderData[val].id;
        },

        okDel(){ //点击删除确认的时候
            this.$http.delete('http://192.168.5.21:8080/store-promotion-info/delete/'+this.id)
            .then((data)=>{
                if(data.data.result == 1){
                    this.$Message.info(data.data.message);
                    this.initTable()
                }else{
                    alert(data.data.message)
                }
            }).catch((err)=>{
                console.log(err)

            })
        },
        cancelDel(){
            this.$Message.info('取消删除');
        },
        initTable(){//初始化页面
            this.$http.post('http://192.168.5.21:8080/store-promotion-info/page',{
                store_id:6666,
                promotion_type:4,
                activity_name:this.searchConName,
                pageNum:this.current,
                pageSize:this.pageSize,
            }).then((data)=>{
                console.log(data);
                this.orderData = data.data.list;
                this.total = data.data.total
            }).catch((err)=>{
                console.log(err);
                
            })
        },
        initEditor(){
            this.editor = new E(this.$refs.editor)
            this.editor.customConfig.onchange = (html) => {
                this.editorContent = html
            }
            // 自定义菜单配置
            this.editor.customConfig.menus = [
                
                'head',  // 标题
                'bold',  // 粗体
                'fontSize',  // 字号
                'fontName',  // 字体
                'italic',  // 斜体
                'underline',  // 下划线
                'strikeThrough',  // 删除线
                'foreColor',  // 文字颜色
                'backColor',  // 背景颜色
                'link',  // 插入链接
                'list',  // 列表
                'justify',  // 对齐方式
                'quote',  // 引用
                'emoticon',  // 表情
                'image',  // 插入图片
                'table',  // 表格
                'undo',  // 撤销
                'redo'  // 重复

            ]
            this.editor.customConfig.uploadImgShowBase64 = true
            // 配置服务器端地址
            // editor.customConfig.uploadImgServer = '/upload'
            // 进行下文提到的其他配置

            this.editor.create()
        }

    },
    mounted(){
        this.initTable()

        this.initSeero()
        this.initEditor() //调用富文本
        this.getcommdityList()
    },
    
};
</script>
