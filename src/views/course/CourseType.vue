<template>
    <el-container style="height: 360px">
        <el-aside width="250px">
            <el-tree
                    :data="courseTypes"
                    :props="defaultProps"
                    @node-contextmenu =rightClick
                    accordion>
            </el-tree>
            <div v-show="menuVisible">
                <ul id="menu" class="menu">
                    <li class="menu__item">新增</li>
                    <li class="menu__item">重命名</li>
                    <li class="menu__item">删除</li>
                </ul>
            </div>
        </el-aside>
        <el-main>


        </el-main>
    </el-container>
</template>

<script>
    export default {
        name: "course-type",
        data(){
            return {
                courseTypes: [],
                defaultProps: {
                    children: 'children',
                    label: 'name',
                },
                menuVisible: false,
            }
        },
        methods:{
            /*加载课程类型*/
            getCourseTypes(){
                this.$http.post("/course/courseType/loadTree")
                    .then(res=>{
                        this.courseTypes = res.data;
                    })
            },
            rightClick(MouseEvent, object, Node, VueComponent){
                this.menuVisible=false;
                this.menuVisible=true;
                let menu = document.querySelector('#menu')
                this.styleMenu(menu);
            },
            foo() { // 取消鼠标监听事件 菜单栏
                this.menuVisible = false
                document.removeEventListener('click', this.foo) // 要及时关掉监听，不关掉的是一个坑，不信你试试，虽然前台显示的时候没有啥毛病，加一个alert你就知道了
            },
            handleNodeClick(){

            },
            styleMenu(menu) {
                if (event.clientX > 1800) {
                    menu.style.left = event.clientX - 100 + 'px';
                } else {
                    menu.style.left = event.clientX + 1 + 'px';
                }
                document.addEventListener('click', this.foo); // 给整个document新增监听鼠标事件，点击任何位置执行foo方法
                if (event.clientY > 700) {
                    menu.style.top = event.clientY - 30 + 'px';
                } else {
                    menu.style.top = event.clientY - 10 + 'px';
                }
            }
        },
        mounted(){
            this.getCourseTypes();
        }
    }
</script>

<style scoped>
    .el-aside {
        border: 1px solid #ccc;
        border-right: none;
    }

    .el-main {
        border: 1px solid #ccc;
    }

    .menu__item {
        display: block;
        line-height: 20px;
        text-align: center;
        margin:10px;
        cursor: default;
    }
    .menu__item:hover{
        color: #FF0000;
    }

    .menu {
        height: auto;
        width: auto;
        position: absolute;
        font-size: 14px;
        text-align: left;
        border-radius: 10px;
        border: 1px solid #c1c1c1;
        background-color: #ffffff;
    }

    li:hover {
        background-color: #E0E0E2;
        color: white;
    }
</style>