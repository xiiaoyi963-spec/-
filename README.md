<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
<meta name="screen-orientation" content="landscape">
<meta name="x5-orientation" content="landscape">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<title>三战全能工具箱</title>
<style>
body{margin:0;padding:0;font-family:"Microsoft YaHei",sans-serif;background:#0b1020;color:#e0e6ff;font-size:16px;}
.tab-bar{display:flex;position:fixed;top:0;left:0;right:0;background:#152240;border-bottom:2px solid #304880;z-index:999;}
.tab-bar button{flex:1;padding:12px 4px;background:none;border:none;color:#b0c4ff;font-size:12px;white-space:nowrap;}
.tab-bar button.active{background:#203870;color:#fff;font-weight:bold;}
.page{display:none;padding:70px 15px 25px;}
.page.active{display:block;}
h2{color:#ffd700;text-align:center;font-size:22px;}
h3{color:#ffcc66;font-size:17px;margin:8px 0;}
input,select,button{font-size:14px;padding:6px;margin:4px 0;border-radius:6px;border:none;}
input,select{background:#1a2b50;color:#fff;width:98%;}
button{background:#3060d0;color:#fff;font-weight:bold;}
textarea{width:98%;height:320px;background:#1a2b50;color:#fff;border:none;border-radius:6px;padding:8px;resize:none;}
.card{background:#152240;border:1px solid #304880;border-radius:8px;padding:15px;margin:10px 0;}
.hero-box{display:flex;align-items:center;gap:8px;margin:8px 0;flex-wrap:wrap;}
.hero-avatar{width:40px;height:40px;border-radius:50%;border:2px solid #ffd700;object-fit:cover;background:#223;}
.core-num{color:#ff4444;font-size:18px;font-weight:bold;}
.hero-list{display:flex;flex-wrap:wrap;gap:6px;margin:10px 0;}
.hero-tag{background:#2a3b55;padding:4px 8px;border-radius:4px;font-size:13px;}
.junshi-item{background:#233450;padding:6px;border-radius:5px;margin:5px 0;}
.junshi-type{color:#73d1ff;}
.camp-tab{display:flex;gap:4px;margin:6px 0;}
.camp-tab button{padding:4px 10px;font-size:13px;background:#2a3b55;}
.camp-tab button.active{background:#3060d0;}
.camp-list{display:none;flex-wrap:wrap;gap:4px;margin:6px 0;}
.camp-list.active{display:flex;}
.hero-btn{padding:4px 8px;font-size:13px;background:#2a3b55;border-radius:4px;}
.hero-btn:hover{background:#3060d0;}
.hero-btn.selected{background:#ffd700;color:#000;font-weight:bold;}
.tactics-tab{display:flex;gap:4px;margin:6px 0;flex-wrap:wrap;}
.tactics-tab button{padding:4px 8px;font-size:12px;background:#2a3b55;}
.tactics-tab button.active{background:#ff9900;color:#fff;}
.tactics-list{display:none;flex-wrap:wrap;gap:4px;margin:6px 0;}
.tactics-list.active{display:flex;}
.tactics-btn{padding:4px 7px;font-size:12px;background:#2a3b55;border-radius:4px;}
.tactics-btn:hover{background:#ff9900;}
.tactics-btn.selected{background:#ffd700;color:#000;font-weight:bold;}
</style>
</head>
<body>
<div class="tab-bar">
<button class="active" data-page="page1">建筑计时</button>
<button data-page="page3">简易配队</button>
<button data-page="page4">开荒总览</button>
<button data-page="page5">开荒攻略</button>
<button data-page="page6">铜矿坐标</button>
<button data-page="page7">全战法库</button>
<button data-page="page8">武将仓库</button>
<button data-page="page9">军师技</button>
<button data-page="page10">战局测评</button>
</div>
<script>
const campData = {
    wei:["曹操","SP曹操","司马懿","郭嘉","SP郭嘉","荀彧","SP荀彧","荀攸","程昱","贾诩","满宠","郝昭","张辽","夏侯渊","夏侯惇","乐进","SP乐进","典韦","许褚","SP许褚","张春华","王元姬","王异","钟会","邓艾","曹仁","于禁","张郃","庞德","SP庞德","曹纯","徐晃","王双","曹真","SP曹真","陈群","曹植","曹丕","甄姬","刘晔","SP刘晔","马钧","SP典韦"],
    shu:["刘备","诸葛亮","SP诸葛亮","关羽","SP关羽","张飞","赵云","马超","SP马超","黄忠","SP黄忠","姜维","庞统","关银屏","关兴","张苞","魏延","法正","SP法正","马岱","陈到","徐庶","王平","黄月英","严颜","高览","蒋琬","费祎","伊籍","廖化"],
    wu:["孙权","陆逊","周瑜","SP周瑜","吕蒙","SP吕蒙","太史慈","周泰","甘宁","程普","孙尚香","凌统","孙坚","SP孙坚","鲁肃","黄盖","孙策","马忠","陆抗","诸葛恪","步练师","SP步练师","大乔","小乔","张昭","张纮"],
    qun:["吕布","张角","袁绍","SP袁绍","董卓","SP董卓","貂蝉","SP貂蝉","华佗","左慈","于吉","李儒","高顺","文丑","颜良","公孙瓒","田丰","许攸","沮授","陈宫","袁术","邹氏","蔡文姬","董白","张让","木鹿大王","孟获","祝融夫人","兀突骨","吕玲绮","朵思大王","麴义","华雄","马腾","SP朱儁","SP皇甫嵩","SP卢植","SP张宝","SP张梁","蔡邕","司马徽"]
};
const tacticsData = {
    zhihui:{S:["暂避其锋","盛气凌敌","抚辑军民","铁骑驱驰","用武神通","横戈跃马","形一阵","转·形一阵","衡轭阵","蓄势待发","万军夺帅"],A:["御敌屏障","舌战群儒","挫锐","梦中弑臣","义心昭烈"],B:["坚守","铁壁","坐守"]},
    zhudong:{S:
