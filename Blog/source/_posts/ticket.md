---
date: 2019-08-03 09:17
title: snh48抢票
categories: 
- 技术
---

## 抢票
url 格式为 `http://shop.48.cn/tickets/item/*`

根据github上的教程改编而来
```
// ==UserScript==
// @name         SNH48抢票脚本
// @version      0.2
// @description  SNH48新官方商城抢票脚本
// @match        https://shop.48.cn/tickets/item/*
// @grant        none
// ==/UserScript==

$(function() {
    'use strict';

    console.info('===>link start!');

    //设置购买数量
    var _num=1;
    //设置购买座位  2vip 3普 4站
    var _seattype=3;
    //设置循环间隔
    var _time= 2000;



    var _url= "/TOrder/add"; // 下单链接
    var _id=location.pathname.split('/tickets/item/')[1]; // 商品id
    let brandStr = $("script").eq(10).text().match(/_brand_id = \"(\d+)\"/g)[0] // 包含有brand_id的脚本

    let _brand_id = parseInt(brandStr.split('"')[1])

    var _choose_times_end = $("#choose_times_end").length == 0 ? -1 : parseInt($("#choose_times_end").val());

    function _loop(){
        layer.msg('抢票中 0rz...');
        setTimeout(function(){tickets();},_time);
    }


    function tickets(){
        var __id =_id;
        var __url= "/TOrder/tickCheck";
        $.ajax({
            url: __url,
            type: "post",
            dataType: "json",
            data: { id: __id,r: Math.random() },
            success: function (result) {
                if (result.HasError) {
                    _loop();
                }
                else
                {
                    switch(result.ErrorCode)
                    {
                        case "success":
                            window.location.href = result.ReturnObject;
                            break;
                        case "fail":
                            layer.msg(result.Message);
                            break;
                        default:
                            _loop();
                    }

                }
            },
            error: function (e) {
                _loop();

            }
        });
    }


    // 下单
    function init(){
        console.log('url', _url)
        console.log(_id, _num, _seattype,_brand_id)
        $.ajax({
            url: _url,
            type: "post",
            dataType: "json",
            data: {
                id: _id,            // 商品id
                num: _num,          // 数量
                seattype:_seattype, // 座位类型
                brand_id: _brand_id,
                r: Math.random(),
                choose_times_end: _choose_times_end
            },
            success: function (result) {
                if (result.HasError) {
                    //失败操作
                    console.error("error init")
                    layer.msg(result.Message);
                }
                else {
                    if(result.Message =="success")
                    {
                        window.location.href = result.ReturnObject;
                    }else
                    {
                        _loop();
                    }
                }
            },
            error: function (jqXHR, textStatus, errorThrown) {
                console.log(jqXHR, textStatus, errorThrown)
               layer.msg("下单异常,请刷新重试");
            }
        });
    }

    init();

    console.info('===>link block!');
});
```
## 切冷餐
url 格式为 `https://shop.48.cn/goods/item/*`

如果抢到票会跳转到 `https://shop.48.cn/order/buy`

因此我们需要针对两个页面分布写两个脚本
```
// ==UserScript==
// @name         SNH48切冷餐脚本1
// @namespace
// @version      1.0.0
// @author       kunkun
// @match        https://shop.48.cn/goods/item/*
// @grant        none
// ==/UserScript==


$(function() {
    'use strict';

    console.info('===>脚本1 start!');

    // 冷餐购买链接
    var limitUrl = "/order/limited"

    // 冷餐id
    var goodsId = location.pathname.split('/goods/item/')[1]

    var postData = {
        goods_id: goodsId, // 冷餐id，根据不同商品切换
        attr_id: 0,
        num: 1,
        isLoadCardNum: true,
        brand_id: 1
    }

    // 抢票间隔
    var timeInterval = 10 // 10ms 一次请求大概2ms
    // 计时器
    var timer = null

    function _loop(){
        layer.msg('抢票中 0rz...');
        timer = setTimeout(function(){init();}, timeInterval);
    }


    // 购买
    function buy(){
        $.ajax({
            url: limitUrl,
            type: "post",
            dataType: "json",
            data:  postData,
            success: function (result) {
                if (result.HasError) {
                    //失败操作
                    _loop();
                    layer.msg(result.Message);
                }
                else {
                    if(result.Message =="success") {
                        window.clearTimeout(timer);
                        // 成功则跳转到 https://shop.48.cn/order/buy
                        window.location.href = result.ReturnObject;
                    } else {
                        _loop();
                    }
                }
            },
            error: function (jqXHR, textStatus, errorThrown) {
               // console.log(jqXHR, textStatus, errorThrown)
               layer.msg("下单异常,请刷新重试");
            }
        });
    }

    buy();
    console.info('===>脚本1 结束!');
});
```

```
// ==UserScript==
// @name         SNH48切冷餐脚本2
// @namespace
// @version      1.0.0
// @author       kunkun
// @match        https://shop.48.cn/order/buy
// @grant        none
// ==/UserScript==


$(function() {
    'use strict';

    console.info('===>脚本2 start!');

    // 提交订单链接
    var submitUrl = "/Order/BuySaveForGive"

    // 检查排队情况链接
    var checkUrl = "/order/orderCheck"

    var submitData = {
        AddressID: $("#AddressID").val(),
        goods_amount: $("#goods_amount").val(),
        shipping_fee: $("#shipping_fee").val(),
        goods_id: $("#goods_id").val(),
        attr_id: $("#attr_id").val(),
        num: $("#num").val(),
        sumPayPrice: $("#sumPayPrice").val(),
        is_inv: $("#chkInvoice").is(':checked'),
        lgs_id: $("#lgs_id").val(),
        integral: $("#integral").val(),
        invoice_type:  $('#invoice_type').val(),
        invoice_title: $("#invoice_title").val(),
        invoice_price: $("#invoicePrice").val(),
        CompanyName: $("#CompanyName").val(),
        TaxpayerID: $("#TaxpayerID").val(),
        CompanyAddress: $("#CompanyAddress").val(),
        CompanyPhone: $("#CompanyPhone").val(),
        CompanyBankOfDeposit: $("#CompanyBankOfDeposit").val(),
        CompanyBankNo: $("#CompanyBankNo").val(),
        rule_goodslist_content: "",
        radom_rule_goodslist_content: "",
        remark: "",
        IsIntegralOffsetFreight: $("#IsIntegralOffsetFreight").val(),
        r: Math.random()
    }

    // 提交间隔
    var timeInterval = 1000 // 1s 一次请求大概2ms

    // 计时器
    var timer = null
    var timer2 = null

    var layerIndex = layer.load();

    // 下单循环
    function _loop(){
        console.log('下单中 0rz...');
        timer = setTimeout(function(){submitOrder();}, timeInterval);
    }

    // 排队循环
    function checkLoop() {
        console.log('排队中 0rz...');
        timer2 = setTimeout(function(){submitOrder();}, timeInterval);
    }

    // 检查排队情况
    function check() {
        var goods_id = $("#goods_id").val();
        $.ajax({
            url: checkUrl,
            type: "GET",
            dataType: "json",
            data: {
                id: goods_id,
                r: Math.random()
            },
            success: function(result) {
                if (result.HasError) {
                    layer.msg(result.Message);
                    // 重新提交订单
                    submitOrder();
                } else {
                    switch (result.ErrorCode) {
                        case"fail":
                            layer.msg(result.Message);
                            checkLoop();
                            break;
                        case"success":
                            // 跳转到结算
                            window.location.href = result.ReturnObject;
                            break;
                        default:
                            checkLoop();
                    }
                }
            },
            error: function (e) {
                checkLoop();

            }
        });
    }
    // 下单
    function submitOrder() {
         $.ajax({
            url: submitUrl,
            type: "post",
            dataType: "json",
            data: submitData,
            success: function(result) {
                if (result.HasError) {
                    layer.close(layerIndex);
                    layer.msg(result.Message + "<br/>错误代码:" + result.ErrorCode);
                    _loop();
                } else {
                    if (result.Message == "wait") {
                        // 检查排队情况 还是重新下单？
                        check();
                    } else {
                        // 跳转到结算
                        location.href = result.ReturnObject;
                    }
                }

            },
            error: function(e) {
                _loop();
            }
        });
    }

    submitOrder();
    console.info('===>脚本2 结束!');
});
```

[参考资料](https://github.com/fanpaa/SNH48-Get-Ticket-For-48cn)