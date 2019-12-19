# DataGrid
## 表格选项
collapsible - 表格是否可收起


## Demo 
1. 数据表格选择
这个调用方式有点意思。
    
    var row = $('#dg').datagrid('getSelected');

2. 行边界
就是设置 td 的 css。有这样一个列子也好，省的需要时在自己去分析页面结构了。

    <style type="text/css">
        .lines-both .datagrid-body td{
        }
        .lines-no .datagrid-body td{
            border-right:1px dotted transparent;
            border-bottom:1px dotted transparent;
        }
        .lines-right .datagrid-body td{
            border-bottom:1px dotted transparent;
        }
        .lines-bottom .datagrid-body td{
            border-right:1px dotted transparent;
        }
    </style>

3. 数据表格复选框选择

下面这两个属性挺绕的。用默认值就好。


    checkOnSelect   boolean 如果为true，当用户点击行的时候该复选框就会被选中或取消选中。
    如果为false，当用户仅在点击该复选框的时候才会被选中或取消。
    （该属性自1.3版开始可用）  true

    selectOnCheck   boolean 如果为true，单击复选框将永远选择行。
    如果为false，选择行将不选中复选框。
    （该属性自1.3版开始可用）  
4. 数据表格工具栏
工具栏的事件处理函数, this 是对应的按钮。

    var toolbar = [{
        text:'Add',
        iconCls:'icon-add',
        handler:function(){
            console.log(this);
            alert('add');
        }
    },{
        text:'Cut',
        iconCls:'icon-cut',
        handler:function(){alert('cut')}
    },'-',{
        text:'Save',
        iconCls:'icon-save',
        handler:function(){alert('save')}
    }];
5. 数据表格复杂工具栏
果然支持模板。还是用这个好。

6. 数据表格过滤行
这个要插件，先不管，90%也用不到。

7. Custom DataGrid Pager
没啥意思。

9. DataGrid Pagination Demo
它这个原生获取数据，返回数据结构还是很简单的。

和分页相关的请求参数就如下两个：
page: 2
rows: 10

页码从1开始。

total 是总数据行数

    {
        "rows": [
            {
                "attr1": "Adult Male",
                "itemid": "EST-19",
                "listprice": "15.50",
                "productid": "AV-SB-02",
                "status": "P",
                "unitcost": "2.00"
            }
        ],
        "total": "28"
    }
8. Client Side Pagination in DataGrid
太麻烦，一般也用不到。先不看了。

9. Sorting
默认时远程排序。先关选项是 remoteSort。
排序相关的请求参数。

sort: itemid
order: asc


10. Multiple Sorting
先不够用管。

11. Frozen Columns in DataGrid
这个做法和 layui 差别比较大。感觉还是 layui 好。

12. Format DataGrid Columns
没 layui 支持的方式多。

    <th data-options="field:'listprice',width:80,align:'right',formatter:formatPrice">List Price</th>

    function formatPrice(val,row){
        if (val < 30){
            return '<span style="color:red;">('+val+')</span>';
        } else {
            return val;
        }
    }

13. Frozen Rows in DataGrid
调用 freezeRow，函数时行索引，索引从0开始。
$(this).datagrid('freezeRow',2)

    
14. Row Editing in DataGrid
太麻烦，先不看
15. Cell Editing in DataGrid
同上
16. Cache Editor for DataGrid
同上
17. DataGrid Row Style

    data-options="
        singleSelect: true,
        iconCls: 'icon-save',
        url: 'datagrid_data1.json',
        method: 'get',
        rowStyler: function(index,row){
            if (row.listprice < 30){
                return 'background-color:#6293BB;color:#fff;font-weight:bold;';
            }
        }
    "
18. Footer Rows in DataGrid
这个有点意思，要服务器处理的。

    {
        "footer": [
            {
                "listprice": 60.4,
                "productid": "Average:",
                "unitcost": 19.8
            },
            {
                "listprice": 604.0,
                "productid": "Total:",
                "unitcost": 198.0
            }
        ],
        "rows": [
            {
                "attr1": "Large",
                "itemid": "EST-1",
                "listprice": 36.5,
                "productid": "FI-SW-01",
                "status": "P",
                "unitcost": 10.0
            }
        ],
        "total": 28
    }

19. Merge Cells for DataGrid
没啥意思
    
    $(this).datagrid('mergeCells',{
        index: merges[i].index,
        field: 'productid',
        rowspan: merges[i].rowspan
    });
20. Context Menu on DataGrid
先不管

21. Expand row in DataGrid to show details
没啥意思，还需要插件datagrid-detailview.js

22. Expand row in DataGrid to show subgrid
同上

33. Loading nested subgrid data
同上

34. Large Data
这个比较奇怪的是，返回的数据是下面这样的

    [
        {
            "amount": 72,
            "cost": "9252.00",
            "date": "2019-12-19",
            "inv": "INV0001",
            "name": "Name1",
            "note": "Note1",
            "price": "128.50"
        },
        {
            "amount": 69,
            "cost": "7180.14",
            "date": "2019-12-20",
            "inv": "INV0002",
            "name": "Name2",
            "note": "Note2",
            "price": "104.06"
        }
    ]

35. Fluid DataGrid
没啥说的

    <th data-options="field:'listprice',align:'right',resizable:false" width="15%">List Price(15%)</th>

36.Virtual Scroll View
分页的另外一种模式，也没啥意思

37. DataGrid Buffer View Demo
看不出来和上面有啥区别

38. group view
就是根据某个字段将服务器返回的数据进行分组显示。没啥意思