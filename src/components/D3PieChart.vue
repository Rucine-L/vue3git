<template>
  <h1>2023五一假期各区域热度分布</h1>
  <h2>数据来源——2023五一旅游大数据报告-马蜂窝</h2>
  <h3>李心茹</h3>
  <div class="D3pie"></div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'D3PieChart',
  data() {
    return {
       w : window.innerWidth
              || document.documentElement.clientWidth
              || document.body.clientWidth,
       h : window.innerHeight
              || document.documentElement.clientHeight
              || document.body.clientHeight,
       dataset:[["东北",4],["西北",8],["华中",10],["华北",11],
          ["华南",14],["西南",18],["华东",35]],
       colors : ["#6FCEDD", "#9EF4E5", "#E2FDBF", "#9AEBA3", "#00ADAE","#67BEAD","#82D9B0"]
    };
},
  mounted() {
          var width = this.w * 0.98;
          var height = this.h * 0.98;
          //转化数据为适合生成饼图的对象数组
        var pie = d3.pie()
                  .value(function(d){return d[1];}).sort(null);
        
        var innerRadius = 40;//圆环内半径
        
        var arc=d3.arc()//设置弧度的内外半径，等待传入的数据生成弧度
                  .innerRadius(innerRadius)
                  .outerRadius(function (d) {
                    var value=d.value;
                    return value*6+ innerRadius;
                  })
                   // 圆角
                  .cornerRadius(16);		

        var svg=d3.select("body")
                  .append("svg")
                  .attr("width",width)
                  .attr("height",height);

        var color = this.colors;
        
        //准备分组,把每个分组移到图表中心
        var arcs=svg.selectAll("g")
                    .data(pie(this.dataset))
                    .enter()
                    .append("g")
                    //移到图表中心
                    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");//translate(a,b)a表示横坐标起点，b表示纵坐标起点

        //为组中每个元素绘制弧形路路径
        arcs.append("path")//每个g元素都追加一个path元素用绑定到这个g的数据d生成路径信息
            .attr("fill",function(d,i){//填充颜色
                return color[i];
                })
            .attr("d",arc)//将角度转为弧度（d3使用弧度绘制）
            .on('mouseover',function(d,i){
                d3.select(this)
                  .transition() // 添加过渡效果
                  .duration(100) // 过渡持续时间
                  .attr("fill", "#FF9300");	
                d3.select("#location"+i.index)
                  .attr("fill", "#FF9300");
                d3.select("#location_name"+i.index)
                  .attr("font-size", "15px")
                  .attr("font-weight", "bold");
                svg.append("text")
                  .attr("id","value")
                  .attr("x",width/2+1)
                  .attr("y",height/2+9)
                  .attr("fill", "#0f5c8c")
                  .attr("text-anchor","middle")
                  .attr("font-size",25)
                  .text(i.value+"%");
                d3.select("#location_text"+i.index)
                .attr("display","block")
                  
                  })
            .on('mouseout',function(d,i){
              d3.select(this)
                .transition()
                .duration(500)
                .attr("fill",color[i.index]);	
              d3.select("#location"+i.index)
                .attr("fill", color[i.index]);
              d3.select("#location_name"+i.index)
                .attr("font-size", "12px")
                .attr("font-weight", "none");
              d3.select("#value")
                .remove();
              d3.select("#location_text"+i.index)
                .attr("display","none")
          });	
          arcs.append("text")
              .attr("id",function(d){return "location_text"+d.index;})
              .attr("transform",function(d){
                  var x = arc.centroid(d)[0];
                  var y = arc.centroid(d)[1];
                  return "translate(" + x + "," + y + ")";
                  })
              .attr("text-anchor","middle")
              .attr("fill", "#0f5c8c")
              .attr("font-size",function(d) { 
                    return d.data[1]*0.8 + "px"; 
                  })
              .attr("font-weight","bold")
              .attr("display","none")
              .text(function(d){				
                   return d.data[0];
                  });
         //设置图注
          for (var i=0;i<7;i++){
                svg.append("rect")
                  .attr("id","location"+i)
                  .attr("x",900)   
                  .attr("y", 50+i*30)  
                  .attr("width", 20)   
                  .attr("height", 20)
                  .attr("rx", 6) // 设置水平方向的圆角半径
                  .attr("ry", 6) // 设置垂直方向的圆角半径   
                  .attr("fill", this.colors[i]);  
                svg.append("text")
                  .attr("id","location_name"+i)
                  .attr("fill", "#0f5c8c")
                  .attr("font-size", "12px")
                  .attr("x",930)
                  .attr("y", 65+i*30)
                  .text(this.dataset[i][0]);
              }
    }
 }
</script>

<style scoped>
      .D3pie{
          margin: 0px;
          display: flex;
          justify-content: center;
          align-items: center;
      }
      h1{
          color: #0f5c8c;
          padding: 0px;
          margin: 0px;
          display: flex;
          justify-content: center;
          align-items: center;
      }
      h2{
        color: #0d669e;
          padding: 5px;
          margin: 0px;
          display: flex;
          justify-content: center;
          align-items: center;
      }
      h3{
        color: #0d669e;
          padding: 3px;
          margin: 0px;
          display: flex;
          justify-content: center;
          align-items: center;
      }
</style>
