<template>
    <div class="dataentry">
        <el-row class="rows" :gutter="20">
            <el-col :span="5">
                <div class="left-tree">
                    <el-tree :data="tree" node-key="label" default-expand-all :expand-on-click-node="false">
                        <div slot-scope="{ node, data }" :title="node.label"
                            :class="'custom-tree-node tree-node-' + node.data.zindex">
                            <el-input class="tree-input-change" size="mini" v-if="node.data.zindex == 8"
                                @change="treeInputChange(node)" v-model="addTreeText"></el-input>
                            <p v-else class="custom-tree-node-text">{{ node.label }}</p>
                            <span style="float:right;margin-right:30px">
                                <i @click="addTree(node)"
                                    :class="node.data.zindex == 2 ? 'tree-icon el-icon-circle-plus-outline' : ''"></i>
                                <span v-if="node.data.zindex == 2" class="badge">{{ node.data.children.length }}</span>
                                <i @click="delTree(node)"
                                    :class="node.data.zindex == 3 ? 'tree-icon el-icon-delete' : ''"></i>
                            </span>
                        </div>
                    </el-tree>
                </div>
            </el-col>
            <el-col :span="19">
                <el-card class="box-card patientInfo">
                    <div slot="header" class="clearfix">
                        <span>病人一般情况</span>
                    </div>
                    <div>
                        <el-form :inline="true" label-width="150px" :model="patientInfo">
                            <el-form-item label="姓名:">
                                <el-input style="width:200px" size="mini" v-model="patientInfo.name"></el-input>
                            </el-form-item>
                            <el-form-item label="距离上次手术时间:">
                                <el-date-picker style="width:200px" size="mini" v-model="patientInfo.operativeTime"
                                    type="month" placeholder="选择月">
                                </el-date-picker>
                            </el-form-item>
                            <el-form-item label="有无微信?:">
                                <el-checkbox v-model="patientInfo.vchat"></el-checkbox>
                            </el-form-item>
                            <el-form-item label="既往就诊医院:">
                                <el-select style="width:200px" size="mini" v-model="patientInfo.hospital" placeholder="请选择">
                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                        :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="住院号:">
                                <el-input style="width:200px" size="mini" v-model="patientInfo.code"></el-input>
                            </el-form-item>
                        </el-form>
                    </div>
                </el-card>
                <el-card class="box-card imageInfo">
                    <div slot="header" class="clearfix">
                        <span>影像学检查 - 2024-01-01-name</span>
                    </div>
                    <div>
                        <el-form label-width="150px" :model="imageInfo">
                            <el-row>
                                <el-col :span="8">
                                    <el-form-item label="影像手段:">
                                        <el-input style="width:200px" size="mini" v-model="patientInfo.name"></el-input>
                                    </el-form-item>

                                    <el-form-item label="本院影像:">
                                        <el-select style="width:200px" size="mini" v-model="patientInfo.hospital"
                                            placeholder="请选择">
                                            <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                :value="item.value">
                                            </el-option>
                                        </el-select>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="16">
                                    <el-form-item label="影像补充:" style="width: 100%;">
                                        <el-input type="textarea" size="mini" v-model="patientInfo.name"></el-input>
                                    </el-form-item>

                                </el-col>
                            </el-row>


                        </el-form>
                        <el-form label-position="top" label-width="150px" :model="imageInfo">
                            <el-row class="rowSelect">
                                <el-col :span="10">
                                    <el-row class="left">
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="颅内动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :span="12">
                                            <el-form-item label="右颅内动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :span="12">
                                            <el-form-item label="右颅外动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="右颅总动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="右椎动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="右锁骨下动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="头臂干">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="右腋动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                    </el-row>
                                </el-col>
                                <el-col :span="4" style="text-align: center;">
                                    <img style="width:90%" src="@/assets/images/a12368b55eb1b9a1c6de6d9ca039ea8.png" alt="">
                                </el-col>
                                <el-col :span="10">
                                    <el-row>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="willis环">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :span="12">
                                            <el-form-item label="左颅内动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :span="12">
                                            <el-form-item label="左颅外动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="左颅总动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="左椎动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="左锁骨下动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="头臂干">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                        <el-col :offset="12" :span="12">
                                            <el-form-item label="左腋动脉">
                                                <el-select size="mini" v-model="imageInfo.hospital" placeholder="请选择">
                                                    <el-option v-for="item in options" :key="item.value" :label="item.label"
                                                        :value="item.value">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </el-col>
                                    </el-row>
                                </el-col>
                            </el-row>
                        </el-form>
                    </div>
                </el-card>
            </el-col>
        </el-row>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            tree: [{
                label: '病人一般情况 : 张三 - 20240101199812126666666666666666',
                zindex: 0,
                children: [{
                    label: '病例信息库',
                    zindex: 1,
                    children: [{
                        label: '住院手术信息',
                        zindex: 2,
                        children: [{
                            label: '手术信息-2024-01-01',
                            zindex: 3,
                        }],
                    }, {
                        label: '实验室检验结果',
                        zindex: 2,
                        children: [{
                            label: '手术信息-2024-01-01',
                            zindex: 3,
                        }],
                    },
                    {
                        label: '影像学检查结果',
                        zindex: 2,
                        children: [{
                            label: '手术信息-2024-01-01',
                            zindex: 3,
                        }],
                    },
                    {
                        label: '疾病活动性评分',
                        zindex: 2,
                        children: [{
                            label: '手术信息-2024-01-01',
                            zindex: 3,
                        }],
                    },
                    {
                        label: '质量健康评价',
                        zindex: 2,
                        children: [{
                            label: '手术信息-2024-01-01',
                            zindex: 3,
                        }],
                    },
                    {
                        label: '随访信息',
                        zindex: 2,
                        children: [{
                            label: '手术信息-2024-01-01',
                            zindex: 3,
                        }],
                    }]
                },
                {
                    label: '生物样本库',
                    zindex: 1,
                    children: [{
                        label: '组织石蜡块',
                        zindex: 2,
                        children: []
                    }, {
                        label: '组织白片',
                        zindex: 2,
                        children: []
                    },
                    {
                        label: 'HE染色切片',
                        zindex: 2,
                        children: []
                    },
                    {
                        label: 'IHC染色切片',
                        zindex: 2,
                        children: []
                    },
                    {
                        label: '血液冻存样本',
                        zindex: 2,
                        children: []
                    },
                    {
                        label: '组织冻存样本',
                        zindex: 2,
                        children: []
                    }]
                }]
            }],
            addTreeText:'',
            patientInfo: {
                name: '',
                operativeTime: '',
                vchat: false,
                hospital: '',
                code: ''
            },
            options: [],
            imageInfo: {
                name: '',
                operativeTime: '',
                vchat: false,
                hospital: '',
                code: ''
            }
        };
    },
    methods: {
        addTree(node) {
            node.data.children.push({
                label: 'test',
                zindex: 8
            })
            this.addTreeText = ""
        },
        delTree(node) {
            // 递归找到这个元素，在数组中移除
            const groupEach = (arr, key) => {
                let newData = []
                arr?.map((es, index) => {
                    if (es.label != key) {
                        newData.push(es)
                    }
                    es.children = groupEach(es.children, key)
                })
                return newData
            }

            this.tree = groupEach(this.tree, node.data.label)
            this.$forceUpdate()
        },

        treeInputChange(node){
            node.data.label = this.addTreeText;
            node.data.zindex = 3
        }

    }
};
</script>
  
<style lang="less" scoped >
.dataentry {
    padding: 10px;

    .rows {
        height: calc(100vh - 70px);
        margin: 0 !important;

        >div {
            height: 100%;
        }
    }

    .left-tree {
        // background-color: rgba(217, 217, 217, 1);
        // border: 1px solid rgba(217, 217, 217, 1);
        box-shadow: 0 0 15px 5px rgba(217, 217, 217, .5);
        height: 100%;
        overflow: auto;
        padding: 5px;

        >div {
            color: #333;
            background-color: transparent !important;
        }
    }

    .el-card__header {
        height: 30px;
    }


}
</style>

<style lang="less">
.tree-node-3 {
    .custom-tree-node-text {
        background-color: rgb(205, 197, 197) !important;
        padding: 0 5px;
    }
}

.tree-node-1 {
    .custom-tree-node-text {
        font-weight: 600;
    }
}

.tree-node-2 {
    .custom-tree-node-text {
        font-weight: 500;
        font-style: oblique
    }
}

.custom-tree-node {
    /* float: right; */
    width: 100%;
    // line-height: 27px;
    font-size: 12px;
    overflow: hidden;
    margin-top: 5px;
    height: 18px;

    .tree-input-change ,.tree-input-change input {
        height: 18px;
        line-height: 18px;
       
    }

    &-text {
        max-width: 80%;
        overflow: hidden;
        display: inline-block;
        text-overflow: ellipsis; //溢出用省略号显示
        white-space: nowrap; //溢出不换行
        margin: 0;
        line-height: 18px;
    }

    .tree-icon {
        font-size: 15px;
        margin: 0 3px;
    }

    .hide {
        display: none;
    }

    .badge {
        background: #409eff;
        color: #fff;
        border-radius: 50%;
        width: 15px;
        text-align: center;
        line-height: 15px;
        height: 15px;
        display: inline-block;
        vertical-align: text-top;
        margin-top: -2px;
    }

}

.box-card {
    .el-card__header {
        height: 40px;
        line-height: 40px;
        padding: 0px 10px;
    }

    .el-form-item__label {
        font-weight: 500 !important;
    }

    .el-form-item {
        margin-bottom: 10px;
    }
}

.patientInfo {
    height: 20%;
}

.imageInfo {
    height: calc(80% - 10px);
    margin-top: 10px;

    .rowSelect {
        .el-form-item__label {
            height: 20px;
            line-height: 20px;
        }

        .left .el-form-item {
            text-align: right;
        }
    }

}
</style>