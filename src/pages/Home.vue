<template>
    <div class="container">
		<div id="container" style="height: 100%"></div>
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
	
	var geoCoordMap = {
	    '南京动车运用所':[118.77254,32.103729],
	    '南京南动车运用所':[118.76094,31.945879],
	    '合肥南动车运用所':[117.272538,31.800093],
	    '徐州东动车运用所':[117.320925,34.21357],
	};
	var convertData = function (data) {
	    var res = [];
	    for (var i = 0; i < data.length; i++) {
	        var geoCoord = geoCoordMap[data[i].name];
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
	    bmap: {
	        center: [118.114129, 32.500339],
	        zoom: 8,
	        roam: true,
	        mapStyle: {
	            styleJson: [
	                    {
	                        "featureType": "water",
	                        "elementType": "all",
	                        "stylers": {
	                            "color": "#044161"
	                        }
	                    },
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
	                    {
	                        "featureType": "railway",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    },
	                    {
	                        "featureType": "highway",
	                        "elementType": "geometry",
	                        "stylers": {
	                            "color": "#004981"
	                        }
	                    },
	                    {
	                        "featureType": "highway",
	                        "elementType": "geometry.fill",
	                        "stylers": {
	                            "color": "#005b96",
	                            "lightness": 1
	                        }
	                    },
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
	                    {
	                        "featureType": "label",
	                        "elementType": "all",
	                        "stylers": {
	                            "visibility": "off"
	                        }
	                    }
	            ]
	        }
	    },
	    series : [
	        {
	            name: 'pm2.5',
	            type: 'scatter',
	            coordinateSystem: 'bmap',
	            data: convertData(data),
	            symbolSize: function (val) {
	                return val[2] / 10;
	            },
	            label: {
	                normal: {
	                    formatter: '{b}',
	                    position: 'right',
	                    show: false
	                },
	                emphasis: {
	                    show: true
	                }
	            },
	            itemStyle: {
	                normal: {
	                    color: '#ddb926'
	                }
	            }
	        },
	        {
	            name: 'Top 5',
	            type: 'effectScatter',
	            coordinateSystem: 'bmap',
	            data: convertData(data.sort(function (a, b) {
	                return b.value - a.value;
	            }).slice(0, 6)),
	            symbolSize: function (val) {
	                return val[2] / 10;
	            },
	            showEffectOn: 'emphasis',
	            rippleEffect: {
	                brushType: 'stroke'
	            },
	            hoverAnimation: true,
	            label: {
	                normal: {
	                    formatter: '{b}',
	                    position: 'right',
	                    show: true
	                }
	            },
	            itemStyle: {
	                normal: {
	                    color: '#f4e925',
	                    shadowBlur: 10,
	                    shadowColor: '#333'
	                }
	            },
	            zlevel: 1
	        },
	        {
	            type: 'custom',
	            coordinateSystem: 'bmap',
	            renderItem: renderItem,
	            itemStyle: {
	                normal: {
	                    opacity: 0.5
	                }
	            },
	            animation: false,
	            silent: true,
	            data: [0],
	            z: -10
	        }
	    ]
	}
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
			var myChartPart2 = echarts.init(document.getElementById('container'));
				// this.initEchart('container', myChartPart2);
				myChartPart2.setOption(option, true);
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
</style>