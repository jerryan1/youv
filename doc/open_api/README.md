<style>

li{list-style: none;}
/* sprite */
.banner .btn-wrap span,.data-stati .item .img,.footer .download .platform a,.footer .download .platform a b,.icon-list b,.about-us .item b{ background:url(../images/sprite.png) no-repeat;}

/* header */
.header{ height:70px; line-height: 70px; background:#fff; border-bottom:1px solid #eee;}
.header .logo img{max-height: 60px; vertical-align: middle;}
.header .logo span{display: inline-block; height: 20px; line-height: 20px; padding-left: 10px; border-left: 1px solid #ddd; margin-left: 10px; font-size: 18px; color: #333945; vertical-align: middle; position: relative; top: 2px;}

/* 支付类型 */
.pay_type{ background:#fff; margin-top:40px; overflow:hidden;}
.pay_type .title{ height:58px; line-height:58px; text-indent:32px; font-size:24px; background:#F5F5F5; border-bottom:1px solid #eee;}
.pay_type .list{ margin:25px auto;}
.pay_type .list li{ float:left; width:210px; height:270px; border:1px solid #eee; margin:15px;}
.pay_type .list li a{ display:block; height:270px; overflow:hidden; padding:0 8px; text-align:center;}
.pay_type .list li a:hover{ background:#f5f5f5; text-decoration:none;}
.pay_type .list li a .ico{ display:inline-block; width:100px; height:100px; background:url(../images/saoma.png) center center no-repeat; margin-top:42px;}
.pay_type .list li a h4{ font-size:14px; font-weight:normal; margin-top:26px;}
.pay_type .list li a p{ color:#bbb; margin-top:8px; line-height:1.6;}

/* 目录 */
.catelog{ margin-top:40px;}
.cat_list{ position:absolute; width:220px; min-height:600px; background:#fff; border:1px solid #eee;}
.cat_list .item{ border-bottom:1px solid #eee;}
.cat_list .item_name{ display:block; height:60px; line-height:60px; padding-left:40px; font-size:16px; background:url(../images/arrow_right.png) 188px center no-repeat;}
.cat_list .item_name:hover{ text-decoration:none;}
.cat_list .item_name.on1{ background-image:url(../images/arrow_top.png);}
.cat_list .sub_item li{ height:40px; line-height:40px;}
.cat_list .sub_item li a{ display:block; padding-left:56px; font-size:14px; color:#666;}
.cat_list .sub_item li a:hover{ background:#eee; text-decoration:none;}
.cat_list .sub_item li a.on2{ background:#75C146; color:#fff;}

.cat_detail{ background:#fff; border:1px solid #eee; margin-left:230px;}
.cat_detail .title{ height:60px; line-height:60px; margin:0 10px; padding:0 5px; font-size:18px; border-bottom:1px solid #eee;}
.cat_detail .detail{ margin:25px 15px;}
.cat_detail .detail .step{ font-size:14px; line-height:26px;}
.cat_detail .detail .download{ margin-top:20px;}
.cat_detail .detail .download a{ color:#0066B2;}
.cat_detail .detail .img_desc{ margin-top:20px; text-align:center;}

/* footer */
.footer_fixed{ height:40px; margin-top:40px;}
.footer{ height:40px; line-height:40px; background:#3D3D3F; color:#999; font-size:12px; margin-top:-40px;}
.footer span{ margin-right:15px;}

/* table样式 */


</style>


<div class="pay_type center">
        	
            <div class="list">
            	<ul>
            		
	                	<li class="selectType" id="2">
	                    	<a href="#/刷卡支付/slotCard">
	                            <b class="ico" style="background:url(../_images/open_api/刷卡.png)  center center no-repeat;"></b> 
	                            <h4>【统一】刷卡支付</h4>
	                       
	                        </a>
	                    </li>
                    
	                	<li class="selectType" id="1">
	                    	<a href="#/扫码支付/scan">
	                            <b class="ico" style="background:url(../_images/open_api/扫码.png)  center center no-repeat;"></b> 
	                            <h4>扫码支付</br>【微信/支付宝/QQ钱包】</h4>
	                           
	                        </a>
	                    </li>
                    
	                	<li class="selectType" id="3">
	                    	<a href="#/公众号支付/wechatOfficialAccounts">
	                            <b class="ico" style="background:url(../_images/open_api/公众号支付.png)  center center no-repeat;"></b> 
	                            <h4>【微信】公众号支付</h4>
	                           
	                        </a>
	                    </li>


						<li class="selectType" id="3">
	                    	<a href="#/wap支付/wap">
	                            <b class="ico" style="background:url(../_images/open_api/H5支付.png)  center center no-repeat;"></b> 
	                            <h4>【微信】H5支付</h4>
	                           
	                        </a>
	                    </li>

						<li class="selectType" id="8">
	                    	<a href="#/服务窗支付/Alipay_H5">
	                            <b class="ico" style="background:url(../_images/open_api/支付宝服务窗.png)  center center no-repeat;"></b> 
	                            <h4>支付宝服务窗</h4>
	                         
	                        </a>
	                    </li>

						<li class="selectType" id="3">
	                    	<a href="#/网银支付/B2C">
	                            <b class="ico" style="background:url(../_images/open_api/网银支付.png)  center center no-repeat;"></b> 
	                            <h4>网银支付</h4>
	                           
	                        </a>
	                    </li>



						<li class="selectType" id="3">
	                    	<a href="#/快捷支付/quickPaySpacil">
	                            <b class="ico" style="background:url(../_images/open_api/快捷支付.png)  center center no-repeat;"></b> 
	                            <h4>快捷支付</h4>
	                           
	                        </a>
	                    </li>

						<li class="selectType" id="3">
	                    	<a href="#/代付接口/defray">
	                            <b class="ico" style="background:url(../_images/open_api/代付.png)  center center no-repeat;"></b> 
	                            <h4>代付</h4>
	                           
	                        </a>
	                    </li>


						<li class="selectType" id="3">
	                    	<a href="#/商户报备/register">
	                            <b class="ico" style="background:url(../_images/open_api/报备.png)  center center no-repeat;"></b> 
	                            <h4>商户报备</h4>
	                           
	                        </a>
	                    </li>


	                    <li class="selectType" id="3">
	                    	<a href="#/商户报备/create">
	                            <b class="ico" style="background:url(../_images/open_api/新报备.png)  center center no-repeat;"></b> 
	                            <h4>商户报备（新）</h4>
	                           
	                        </a>
	                    </li>


	                	<li class="selectType" id="26">
	                    	<a href="#/对账单下载/downloadBill">
	                            <b class="ico" style="background:url(../_images/open_api/dzd_20170619151536.png)  center center no-repeat;"></b> 
	                            <h4>下载对账单</h4>
	                           
	                        </a>
	                    </li>

	                    <li class="selectType" id="3">
	                    	<a href="#/银行查询/banks">
	                            <b class="ico" style="background:url(../_images/open_api/商户支持银行.png)  center center no-repeat;"></b> 
	                            <h4>银行查询</h4>
	                           
	                        </a>
	                    </li>

	                	<li class="selectType" id="3">
	                    	<a href="javacript:vode(0)">
	                            <b class="ico" style="background:url(../_images/open_api/app_20170802.jpg)  center center no-repeat;"></b> 
	                            <h4>【统一】APP支付(暂未开放)</h4>
	                           
	                        </a>
	                    </li>
                    
                    
                </ul>
                <div class="clear"></div>
            </div>
        </div>