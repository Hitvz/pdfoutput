<template>
    <div>
        <div id="toolBox">
            <div class="toolBox">
                <el-button type="primary" plain size="small" style="margin-top: 20px;width: 130px;" @click="saveAsPdf">
                    打印PDF
                </el-button>
            </div>
        </div>
        <div :id="tableConfig.pdfId" :class="tableConfig.pdfClass" v-for="(tableConfig,configIndex) in tableConfigList"
             :key="configIndex">
            <div class="thisPage">
                <h1 style="text-align: center">window.print方法demo</h1>
            </div>
            <div class="thisPage" v-for="(data,index) in dataList" :key="index" :style="tableConfig.tableStyle">
                <el-table :data="data" :header-cell-style="tableHeader" border>
                    <el-table-column column-key="col1_" prop="col1_" label="第一列"></el-table-column>
                    <el-table-column column-key="col2_" prop="col2_" label="第二列"></el-table-column>
                    <el-table-column column-key="col3_" prop="col3_" label="第三列"></el-table-column>
                    <el-table-column column-key="col4_" prop="col4_" label="第四列"></el-table-column>
                    <el-table-column column-key="col5_" prop="col5_" label="第五列"></el-table-column>
                </el-table>
                <el-divider></el-divider>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'windowPrint',
        data() {
            return {
                data: {
                    col1_: '',
                    col2_: '',
                    col3_: '',
                    col4_: '',
                    col5_: ''
                },
                dataList: [],
                tableHeader: {
                    'color': 'aliceblue',
                    'background-color': 'slateblue'
                },
                tableConfigList: [{
                    pdfId: 'comment_report',
                    pdfClass: '',
                    tableStyle: `'100%'`
                }, {
                    pdfId: 'print_report',
                    pdfClass: 'comment_hide',
                    tableStyle: `width:1100px;`
                }]
            }
        }, methods: {
            getData() {
                let keys = Object.keys(this.data);
                let size = 35;
                for (let i = 0; i < size; i++) {
                    this.dataList[i] = new Array();
                    for (let j = 0; j < i; j++) {
                        let tempData = {};
                        keys.forEach(key => {
                            tempData[key] = key + i + j;
                        })
                        this.dataList[i][j] = tempData;
                    }
                }
                this.dataList[size] = [];
            },
            saveAsPdf() {
                let commentReport = document.getElementById("comment_report");
                let printReport = document.getElementById("print_report");
                let toolBox = document.getElementById("toolBox")
                toolBox.className = "print_hide";
                commentReport.className = "print_hide";
                printReport.className = "print_content";

                let thisPage = printReport.querySelectorAll('.thisPage');
                let curHeight = 0;
                let a3PageHeight = 1558;
                for (let item of thisPage) {
                    let contentHeight = parseInt(window.getComputedStyle(item).height)
                    if ((curHeight + contentHeight) > a3PageHeight) {
                        console.log("a page")
                        item.style.pageBreakBefore = "always";
                        // 清空
                        curHeight = 0;
                    }
                    if(contentHeight<a3PageHeight){
                        curHeight += contentHeight
                    }else {
                        curHeight = contentHeight % a3PageHeight
                    }
                    console.log("item", contentHeight, curHeight);
                }
                setTimeout(() => {
                        window.print();
                        toolBox.className = "toolBox";
                        printReport.className = "comment_hide";
                    }, 1000
                )
            }
        },
        created() {
            this.getData();
        }
    }
</script>

<style scoped>
    .toolBox {
        position: fixed;
        top: 0;
        right: 0;
        width: 100%;
        height: 50px;
        line-height: 50px;
        text-align: right;
        background: rgba(0, 0, 0, 0.3);
        box-shadow: 2px 2px 3px #e4e4e4;
        z-index: 9999;
    }

    .toolBox .el-button {
        margin: 0;
        transform: translate(-50%, -40%);
    }

    @page {
        size: A3;
        margin: 0;
    }

    .comment_hide {
        display: none;
    }

    @media print {

        .print_hide {
            display: none;
        }

        .print_content {
            margin-top: 20px !important;
            -webkit-print-color-adjust: exact;
            -moz-print-color-adjust: exact;
            -ms-print-color-adjust: exact;
            print-color-adjust: exact;
        }
    }
</style>
