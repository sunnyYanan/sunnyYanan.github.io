---
layout: post
title:  datatable中的插件tabletools使用小结
categories: 前端开发
tag: datatable
---

* content
{:toc}

简介：
--------
作为jquery的表格插件，[datatable](https://datatables.net/)提供了方便的分类组合，搜索查找，各列排序等功能，是一个非常好的表格展示插件。

datatable本身还提供了丰富的插件，使表格操作更智能。tabletools就是其中之一。

一个简单的使用例子如[官方](https://www.datatables.net/extensions/tabletools/)，js初始化如下：

```
//Example initialisation
$(document).ready( function () {
    $('#example').dataTable( {
        "dom": 'T<"clear">lfrtip',
        "tableTools": {
            "sSwfPath": "/swf/copy_csv_xls_pdf.swf"
        }
    } );
} );
```

作者在使用过程中遇到的问题有：

1、按钮点击无效，原因是swf路径问题，更改为：
```
$('#example').DataTable( {
        "dom": 'T<"clear">lfrtip',
        "tableTools": {
            "sSwfPath": "//cdn.datatables.net/tabletools/2.2.4/swf/copy_csv_xls.swf",
            "aButtons": [ "xls" ]
        }
    });
```

2、button自定义以及增加描述，设置abuttons属性。如：
```
"aButtons":[
    {"sExtends":"copy", "sButtonText":"拷贝到粘贴板"},
    {"sExtends":"xls", "sButtonText":"导出Excel"},
    {"sExtends":"print", "sButtonText":"打印预览"}
]
```

附一个比较好的自定义按钮学习推荐：[http://phpflow.com/php/how-to-export-the-jquery-datatable-data-to-pdf-excel-csv-and-copy/](http://phpflow.com/php/how-to-export-the-jquery-datatable-data-to-pdf-excel-csv-and-copy/)
