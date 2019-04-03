<template>
    <div class="container">
		<div style="width: 50%;height: 600px;float: left;">
			<div id="container" style="height: 100%"></div>
		</div>
		<div style="width: 50%;float: left;height: 500px;">
			<div id="right1" style="height:200px ;"></div>
			<div id="right2" style="height:200px ;"></div>
			<div id="right3" style="height:200px ;">
				<div class="item"></div>
				<div class="item"></div>
				<div class="item"></div>
			</div>
		</div>
		
    </div>
</template>
<script>
	import echarts from 'echarts'
	require('echarts/extension/bmap/bmap') //引入地图文件
	var option = null;
		var data = [
	    {name: '南京动车运用所', value: 100},
	    {name: '南京南动车运用所', value: 100},
	    {name: '合肥南动车运用所', value: 100},
	    {name: '徐州东动车运用所', value: 100},
	];
	
	
	var optionRight1 = {
    tooltip: {
        trigger: 'item',
        formatter: "{a} <br/>{b}: {c} ({d}%)"
    },
    legend: {
        orient: 'vertical',
        x: 'left',
        data:['扇A','扇B','扇C','扇D','扇E']
    },
    series: [
        {
            name:'扇形统计',
            type:'pie',
            radius: ['50%', '70%'],
            avoidLabelOverlap: false,
            label: {
                normal: {
                    show: false,
                    position: 'center'
                },
                emphasis: {
                    show: true,
                    textStyle: {
                        fontSize: '16',
                        fontWeight: 'bold'
                    }
                }
            },
            labelLine: {
                normal: {
                    show: false
                }
            },
            data:[
                {value:335, name:'扇A'},
                {value:310, name:'扇B'},
                {value:234, name:'扇C'},
                {value:135, name:'扇D'},
                {value:1548, name:'扇E'}
            ]
        }
    ]
};


var optionRight2 = {
    title: {
        text: '条形统计',
        subtext: '',
		left: 'center',
    },
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'shadow'
        }
    },
    legend: {
        data: ['']
    },
    grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
    },
    xAxis: {
        type: 'value',
        boundaryGap: [0, 0.01]
    },
    yAxis: {
        type: 'category',
        data: ['列A','列B','列C','列D','列E','数值(万)']
    },
    series: [
        {
            name: '2011年',
            type: 'bar',
            data: [18203, 23489, 29034, 104970, 131744, 630230],
			itemStyle: {
                normal: {
                    color: new echarts.graphic.LinearGradient(
                        0, 0, 1, 0,
                        [
                            {offset: 0, color: 'blue'},
                            {offset: 0.33, color: 'yellow'},
                            {offset: 0.66, color: 'orange'},
							{offset: 1, color: 'red'}
                        ]
                    )
                },
                emphasis: {
                    color: new echarts.graphic.LinearGradient(
                        0, 0, 0, 1,
                        [
                            {offset: 0, color: '#2378f7'},
                            {offset: 0.7, color: '#2378f7'},
                            {offset: 1, color: '#83bff6'}
                        ]
                    )
                }
            },
        },
    ]
};

	
	
	
	
	
	var geoCoordMap = {
	    '南京动车运用所':[118.77254,32.103729],
	    '南京南动车运用所':[118.76094,31.945879],
	    '合肥南动车运用所':[117.272538,31.800093],
	    '徐州东动车运用所':[117.320925,34.21357],
	};
	
	
	
	
	var data1 = [
	    {name: '暴雨1', value: 100},
	    {name: '暴雨2', value: 100},
	    {name: '暴雨3', value: 100},
	    {name: '暴雨4', value: 100},
	];
	
	var geoCoordMap1 = {
	    '暴雨1':[117.866361,32.264109],
	    '暴雨2':[117.39723,32.178089],
	    '暴雨3':[118.123923,31.974443],
	    '暴雨4':[118.00894,32.443703],
	};
	
	
	var data2 = [
	    {name: '台风1', value: 100},
	    {name: '台风2', value: 100},
	    {name: '台风3', value: 100},
	    {name: '台风4', value: 100},
	];
	
	var geoCoordMap2 = {
	    '台风':[118.298697,32.642396],
	    '台风2':[117.627196,32.619043],
	    '台风3':[117.907755,32.236748],
	    '台风4':[117.719182,32.357855],
	};
	
	
	
	var convertData = function (data,map) {
	    var res = [];
	    for (var i = 0; i < data.length; i++) {
	        var geoCoord = map[data[i].name];
	        if (geoCoord) {
	            res.push({
	                name: data[i].name,
	                value: geoCoord.concat(data[i].value)
	            });
	        }
	    }
	    return res;
	};
	
	function renderItem(params, api) {
	    var coords = [
	       
	    ];
	    var points = [];
	    for (var i = 0; i < coords.length; i++) {
	        points.push(api.coord(coords[i]));
	    }
	    var color = api.visual('color');
	
	    return {
	        type: 'polygon',
	        shape: {
	            points: echarts.graphic.clipPointsByRect(points, {
	                x: params.coordSys.x,
	                y: params.coordSys.y,
	                width: params.coordSys.width,
	                height: params.coordSys.height
	            })
	        },
	        style: api.style({
	            fill: color,
	            stroke: echarts.color.lift(color)
	        })
	    };
	}
					option={
	    // backgroundColor: '#404a59',
	    title: {
	        text: '',
	        subtext: '',
	        sublink: '',
	        left: 'center',
	        textStyle: {
	            color: '#fff'
	        }
	    },
	    tooltip : {
	        trigger: 'item'
	    },
	    bmap1: {
	        center: [118.114129, 32.500339],
	        zoom: 8,
	        roam: true,    //鼠标拖动
	        mapStyle: {
	            styleJson: [
					// 水的颜色
	                    {
	                        "featureType": "water",
	                        "elementType": "all",
	                        "stylers": {
	                            "color": "#044161"
	                        }
	                    },
						// 陆地颜色
	                    {
	                        "featureType": "land",
	                        "elementType": "all",
	                        "stylers": {
	                            "color": "#004981"
	                        }
	                    },
	                    {
	                        "featureType": "boundary",
	                        "elementType": "geometry",
	                        "stylers": {
	                            "color": "#064f85"
	                        }
	                    },
						// 显示铁路
	                    {
	                        "featureType": "railway",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "on"
	                        }
	                    },
						// 显示高速
	                    {
	                        "featureType": "highway",
	                        "elementType": "geometry",
	                        "stylers": {
	                            "color": "#004981"
	                        }
	                    },
						// 显示公路
// 	                    {
// 	                        "featureType": "highway",
// 	                        "elementType": "geometry.fill",
// 	                        "stylers": {
// 	                            "color": "#005b96",
// 	                            "lightness": 1
// 	                        }
// 	                    },
						// 显示省道国道标志
	                    {
	                        "featureType": "highway",
	                        "elementType": "labels",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    },
	                    {
	                        "featureType": "arterial",
	                        "elementType": "geometry",
	                        "stylers": {
	                            "color": "#004981"
	                        }
	                    },
	                    {
	                        "featureType": "arterial",
	                        "elementType": "geometry.fill",
	                        "stylers": {
	                            "color": "#00508b"
	                        }
	                    },
	                    {
	                        "featureType": "poi",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    },
	                    {
	                        "featureType": "green",
	                        "elementType": "all",
	                        "stylers": {
	                            "color": "#056197",
	                            "visibility": "off"
	                        }
	                    },
	                    {
	                        "featureType": "subway",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    },
	                    {
	                        "featureType": "manmade",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    },
	                    {
	                        "featureType": "local",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    },
	                    {
	                        "featureType": "arterial",
	                        "elementType": "labels",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    },
						// 显示省的轮廓
	                    {
	                        "featureType": "boundary",
	                        "elementType": "geometry.fill",
	                        "stylers": {
	                            "color": "#029fd4"
	                        }
	                    },
	                    {
	                        "featureType": "building",
	                        "elementType": "all",
	                        "stylers": {
	                            "color": "#1a5787"
	                        }
	                    },
						// 显示城市名
	                    {
	                        "featureType": "label",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "on"
	                        }
	                    }
	            ]
	        }
	    },
	    series : [
// 	        {
// 	            name: 'pm2.5',
// 	            type: 'scatter',
// 	            coordinateSystem: 'bmap',
// 	            data: convertData(data),
// 	            symbolSize: function (val) {
// 	                return val[2] / 10;
// 	            },
// 	            label: {
// 	                normal: {
// 	                    formatter: '{b}',
// 	                    position: 'right',
// 	                    show: false
// 	                },
// 	                emphasis: {
// 	                    show: true
// 	                }
// 	            },
// 	            itemStyle: {
// 	                normal: {
// 	                    color: '#ddb926'
// 	                }
// 	            }
// 	        },
// 			{
// 			        type: 'map',
// 			        mapType: '上海',
// 			        label: {
// 			            emphasis: {
// 			                textStyle: {
// 			                    color: '#fff'
// 			                }
// 			            }
// 			        },
// 			        itemStyle: {
// 			            normal: {
// 			                borderColor: '#389BB7',
// 			                areaColor: '#fff',
// 			            },
// 			            emphasis: {
// 			                areaColor: '#389BB7',
// 			                borderWidth: 0
// 			            }
// 			        },
// 			        animation: false
// 			        // animationDurationUpdate: 1000,
// 			        // animationEasingUpdate: 'quinticInOut'
// 			    },




// 	        {
// 	            name: '动车所',
// 	            type: 'effectScatter',
// 	            coordinateSystem: 'bmap',
// 	            data: convertData(data.sort(function (a, b) {
// 	                return b.value - a.value;
// 	            }).slice(0, 6),geoCoordMap),
// 	            symbolSize: function (val) {
// 	                return val[2] / 10;
// 	            },
// 	            showEffectOn: 'emphasis',
// 	            rippleEffect: {
// 	                brushType: 'stroke'
// 	            },
// 	            hoverAnimation: true,
// 	            label: {
// 	                normal: {
// 	                    formatter: '{b}',
// 	                    position: 'right',
// 	                    show: true
// 	                }
// 	            },
// 	            itemStyle: {
// 	                normal: {
// 	                    color: '#f4e925',
// 	                    shadowBlur: 10,
// 	                    shadowColor: '#333'
// 						
// 	                }
// 	            },
// 	            zlevel: 1
// 	        },
// 			
// 			
// 			{
// 			    name: '红色',
// 			    type: 'effectScatter',
// 			    coordinateSystem: 'bmap',
// 			    data: convertData(data1.sort(function (a, b) {
// 			        return b.value - a.value;
// 			    }).slice(0, 6),geoCoordMap1),
// 			    symbolSize: function (val) {
// 			        return val[2] / 10;
// 			    },
// 			    showEffectOn: 'emphasis',
// 			    rippleEffect: {
// 			        brushType: 'stroke'
// 			    },
// 			    hoverAnimation: true,
// 			    label: {
// 			        normal: {
// 			            formatter: '{b}',
// 			            position: 'right',
// 			            show: true
// 			        }
// 			    },
// 			    itemStyle: {
// 			        normal: {
// 			            color: 'red',
// 			            shadowBlur: 10,
// 			            shadowColor: '#333'
// 			        }
// 			    },
// 			    zlevel: 1
// 			},
// 			
// 			
// 			{
// 			    name: '红色',
// 			    type: 'effectScatter',
// 			    coordinateSystem: 'bmap',
// 			    data: convertData(data2.sort(function (a, b) {
// 			        return b.value - a.value;
// 			    }).slice(0, 6),geoCoordMap2),
// 			    symbolSize: function (val) {
// 			        return val[2] / 10;
// 			    },
// 			    showEffectOn: 'emphasis',
// 			    rippleEffect: {
// 			        brushType: 'stroke'
// 			    },
// 			    hoverAnimation: true,
// 			    label: {
// 			        normal: {
// 			            formatter: '{b}',
// 			            position: 'right',
// 			            show: true
// 			        }
// 			    },
// 			    itemStyle: {
// 			        normal: {
// 			            color: 'lightgreen',
// 			            shadowBlur: 10,
// 			            shadowColor: '#333'
// 			        }
// 			    },
// 			    zlevel: 1
// 			},
			
			
// 	        {
// 	            type: 'custom',
// 	            coordinateSystem: 'bmap',
// 	            renderItem: renderItem,
// 	            itemStyle: {
// 	                normal: {
// 	                    opacity: 0.5
// 	                }
// 	            },
// 	            animation: false,
// 	            silent: true,
// 	            data: [0],
// 	            z: -10
// 	        }




                {
                    type: 'map',
                    mapType: 'shanghai',
                    label: {
                        emphasis: {
                            textStyle: {
                                color: '#fff'
                            }
                        }
                    },
                    itemStyle: {
                        normal: {
                            borderColor: '#389BB7',
                            areaColor: '#fff',
                        },
                        emphasis: {
                            areaColor: '#389BB7',
                            borderWidth: 0
                        }
                    },
                    animation: false
                    // animationDurationUpdate: 1000,
                    // animationEasingUpdate: 'quinticInOut'
                }





	    ]
	}
	
	
	
	
	
	
	
	var provinces = ['shanghai', 'hebei','shanxi','neimenggu','liaoning','jilin','heilongjiang','jiangsu','zhejiang','anhui','fujian','jiangxi','shandong','henan','hubei','hunan','guangdong','guangxi','hainan','sichuan','guizhou','yunnan','xizang','shanxi1','gansu','qinghai','ningxia','xinjiang', 'beijing', 'tianjin', 'chongqing', 'xianggang', 'aomen'];
var provincesText = ['上海', '河北', '山西', '内蒙古', '辽宁', '吉林','黑龙江',  '江苏', '浙江', '安徽', '福建', '江西', '山东','河南', '湖北', '湖南', '广东', '广西', '海南', '四川', '贵州', '云南', '西藏', '陕西', '甘肃', '青海', '宁夏', '新疆', '北京', '天津', '重庆', '香港', '澳门'];

// function showProvince() {
//     var name = provinces[currentIdx];
// 
//     // myChart.showLoading();
// 
//     // $.get('vendors/echarts/map/json/province/' + name + '.json', function (geoJson) {
// 
//         // myChart.hideLoading();
// 
//         echarts.registerMap(name, require('echarts/map/json/province/anhui.json'));
// 		let myChartPart = echarts.init(document.getElementById('container'));
//         myChartPart.setOption({} = option
// 		{
//             backgroundColor: '#404a59',
//             title: {
//                 text: provincesText[currentIdx],
//                 left: 'center',
//                 textStyle: {
//                     color: '#fff'
//                 }
//             },
//             series: [
//                 {
//                     type: 'map',
//                     mapType: 'shanghai',
//                     label: {
//                         emphasis: {
//                             textStyle: {
//                                 color: '#fff'
//                             }
//                         }
//                     },
//                     itemStyle: {
//                         normal: {
//                             borderColor: '#389BB7',
//                             areaColor: '#fff',
//                         },
//                         emphasis: {
//                             areaColor: '#389BB7',
//                             borderWidth: 0
//                         }
//                     },
//                     animation: false
//                     // animationDurationUpdate: 1000,
//                     // animationEasingUpdate: 'quinticInOut'
//                 }
//             ]
//         }
// 		);
//     // });
// }








var option3={
            backgroundColor: '#404a59',
            title: {
                text: 'anhui',
                left: 'center',
                textStyle: {
                    color: '#fff'
                }
            },
            series: [
                {
                    type: 'map',
                    mapType: 'anhui',
                    label: {
                        emphasis: {
                            textStyle: {
                                color: '#fff'
                            }
                        }
                    },
                    itemStyle: {
                        normal: {
                            borderColor: '#389BB7',
                            areaColor: '#fff',
                        },
                        emphasis: {
                            areaColor: '#389BB7',
                            borderWidth: 0
                        }
                    },
                    animation: false
                },
				 {
				    type: 'map',
				    mapType: 'jiangsu',
				    label: {
				        emphasis: {
				            textStyle: {
				                color: '#fff'
				            }
				        }
				    },
				    itemStyle: {
				        normal: {
				            borderColor: '#389BB7',
				            areaColor: '#fff',
				        },
				        emphasis: {
				            areaColor: '#389BB7',
				            borderWidth: 0
				        }
				    },
				    animation: false
				}
            ]
        }







var currentIdx = 9;
	
	
	
	
	
	
	
	
    export default{
        data(){







            return{

            }
        },
		methods:{
			initEchart(el, options) {
				echarts.init(document.getElementById(el)).setOption(options, true);
			},
		},
        computed:{
        },
        components:{
        },
		mounted() {
			if (option && typeof option === "object") {
// 			let myChartPart = echarts.init(document.getElementById('container'));
// 				myChartPart.setOption(option, true);
		}
		
		if (option3 && typeof option3 === "object") {
			echarts.registerMap('anhui', require('echarts/map/json/province/anhui.json'));
			// echarts.registerMap('jiangsu', require('echarts/map/json/province/jiangsu.json'));
					let myChartPart = echarts.init(document.getElementById('container'));
						myChartPart.setOption(option3, true);
				}
		
		
		if (optionRight1 && typeof optionRight1 === "object") {
			let myChartPart = echarts.init(document.getElementById('right1'));
				// this.initEchart('container', myChartPart2);
				myChartPart.setOption(optionRight1, true);
		}
		
		if (optionRight2 && typeof optionRight2 === "object") {
			let myChartPart = echarts.init(document.getElementById('right2'));
				// this.initEchart('container', myChartPart2);
				myChartPart.setOption(optionRight2, true);
		}
		
		}
    }
</script>
<style scoped>
    .container{
        height:100%;
        background-color:#e5e9f2;
    }
</style>
<style>
/**
 * 默认尺寸为 600px×400px，如果想让图表响应尺寸变化，可以像下面这样
 * 把尺寸设为百分比值（同时请记得为容器设置尺寸）。
 */
.echarts {
  width: 100%;
  height: 100%;
}
#right3{
	display: flex;
	justify-content:space-around; 
}
#right3 .item{
	width: 30%;
	height: 200px;
	float: left;
	background: rgba(25, 58, 66,0.8);
	border-radius: 8px;
}
	
</style>