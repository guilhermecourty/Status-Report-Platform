<script>

import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

let currentWeek = () => {
  return new Promise((resolve, reject) => {
    var date = new Date();
    date.setHours(0, 0, 0, 0);
    // Thursday in current week decides the year.
    date.setDate(date.getDate() + 3 - (date.getDay() + 6) % 7);
    // January 4 is always in week 1.
    var week1 = new Date(date.getFullYear(), 0, 4);
    // Adjust to Thursday in week 1 and count number of weeks from date to week1.
    var result = 1 + Math.round(((date.getTime() - week1.getTime()) / 86400000 - 3 + (week1.getDay() + 6) % 7) / 7);
    resolve(result);
  })
  .then(res => res)
  .catch(err => console.log(err));
}

let currentFy = () =>{
  return new Promise((resolve, reject) => {
    let fy;
    let date = new Date();
    let month = new Date();
    month.setMonth('3');
    date.getMonth() < month.getMonth() ? fy = date.getFullYear() - 1 : fy = date.getFullYear();
    fy = fy.toString();
    fy = fy.slice(2);
    resolve(fy);
  })
  .then(res => res)
  .catch(err => console.log(err));
}

let week;
let fy;
let date = new Date();
date = date.toDateString();
let user = 'Guilherme';
currentWeek().then(res => res < 10 ? week = `0${res}` : week = res);
currentFy().then(res => fy = res);





am4core.ready(function() {

// Themes begin
am4core.useTheme(am4themes_animated);
// Themes end

var chart = am4core.create("chartdiv", am4plugins_timeline.SpiralChart);
//chart.curveContainer.padding(20,20,20,20);
chart.levelCount = 2;
chart.yAxisRadius = am4core.percent(25);
chart.yAxisInnerRadius = am4core.percent(-25);
chart.startAngle = -90;

var colorSet = new am4core.ColorSet();

chart.data = [ {
  "category": "Module #1",
  "start": "2016-01-10",
  "end": "2016-01-13",
  "color": colorSet.getIndex(0),
  "task": "Gathering requirements"
}, {
  "category": "Module #1",
  "start": "2016-02-05",
  "end": "2016-04-18",
  "color": colorSet.getIndex(0),
  "task": "Development"
}, {
  "category": "Module #2",
  "start": "2016-01-08",
  "end": "2016-01-10",
  "color": colorSet.getIndex(5),
  "task": "Gathering requirements"
}, {
  "category": "Module #2",
  "start": "2016-01-12",
  "end": "2016-01-15",
  "color": colorSet.getIndex(5),
  "task": "Producing specifications"
}, {
  "category": "Module #2",
  "start": "2016-01-16",
  "end": "2016-02-05",
  "color": colorSet.getIndex(5),
  "task": "Development"
}, {
  "category": "Module #2",
  "start": "2016-02-10",
  "end": "2016-02-18",
  "color": colorSet.getIndex(5),
  "task": "Testing and QA"
}, {
  "category": "",
  "task": ""
},{
  "category": "Module #3",
  "start": "2016-01-01",
  "end": "2016-01-19",
  "color": colorSet.getIndex(9),
  "task": "Gathering requirements"
}, {
  "category": "Module #3",
  "start": "2016-02-01",
  "end": "2016-02-10",
  "color": colorSet.getIndex(9),
  "task": "Producing specifications"
}, {
  "category": "Module #3",
  "start": "2016-03-10",
  "end": "2016-04-15",
  "color": colorSet.getIndex(9),
  "task": "Development"
}, {
  "category": "Module #3",
  "start": "2016-04-20",
  "end": "2016-04-30",
  "color": colorSet.getIndex(9),
  "task": "Testing and QA"
}, {
  "category": "Module #4",
  "start": "2016-01-15",
  "end": "2016-02-12",
  "color": colorSet.getIndex(15),
  "task": "Gathering requirements"
},{
  "category": "Module #4",
  "start": "2016-02-25",
  "end": "2016-03-10",
  "color": colorSet.getIndex(15),
  "task": "Development"
}, {
  "category": "Module #4",
  "start": "2016-03-23",
  "end": "2016-04-29",
  "color": colorSet.getIndex(15),
  "task": "Testing and QA"
} ];

chart.dateFormatter.dateFormat = "yyyy-MM-dd";
chart.dateFormatter.inputDateFormat = "yyyy-MM-dd";
chart.fontSize = 11;

var categoryAxis = chart.yAxes.push(new am4charts.CategoryAxis());
categoryAxis.dataFields.category = "category";
categoryAxis.renderer.grid.template.disabled = true;
categoryAxis.renderer.labels.template.paddingRight = 25;
categoryAxis.renderer.minGridDistance = 10;
categoryAxis.renderer.innerRadius = -60;
categoryAxis.renderer.radius = 60;
categoryAxis.renderer.labels.template.disabled = true;

var dateAxis = chart.xAxes.push(new am4charts.DateAxis());
dateAxis.renderer.minGridDistance = 70;
dateAxis.baseInterval = { count: 1, timeUnit: "day" };
dateAxis.renderer.tooltipLocation = 0;
dateAxis.startLocation = -0.5;
dateAxis.renderer.line.strokeDasharray = "1,4";
dateAxis.renderer.line.strokeOpacity = 0.7;
dateAxis.tooltip.background.fillOpacity = 0.2;
dateAxis.tooltip.background.cornerRadius = 5;
dateAxis.tooltip.label.fill = new am4core.InterfaceColorSet().getFor("alternativeBackground");
dateAxis.tooltip.label.paddingTop = 7;

var labelTemplate = dateAxis.renderer.labels.template;
labelTemplate.verticalCenter = "middle";
labelTemplate.fillOpacity = 0.7;
labelTemplate.background.fill = new am4core.InterfaceColorSet().getFor("background");
labelTemplate.background.fillOpacity = 1;
labelTemplate.padding(7,7,7,7);

var series1 = chart.series.push(new am4plugins_timeline.CurveColumnSeries());
series1.columns.template.height = am4core.percent(20);
series1.columns.template.tooltipText = "{task}: [bold]{openDateX}[/] - [bold]{dateX}[/]";

series1.dataFields.openDateX = "start";
series1.dataFields.dateX = "end";
series1.dataFields.categoryY = "category";
series1.columns.template.propertyFields.fill = "color"; // get color from data
series1.columns.template.propertyFields.stroke = "color";
series1.columns.template.strokeOpacity = 0;

var bullet = new am4charts.CircleBullet();
series1.bullets.push(bullet);
bullet.circle.radius = 3;
bullet.circle.strokeOpacity = 0;
bullet.propertyFields.fill = "color";
bullet.locationX = 0;


var bullet2 = new am4charts.CircleBullet();
series1.bullets.push(bullet2);
bullet2.circle.radius = 3;
bullet2.circle.strokeOpacity = 0;
bullet2.propertyFields.fill = "color";
bullet2.locationX = 1;

chart.scrollbarX = new am4core.Scrollbar();
chart.scrollbarX.align = "center"
chart.scrollbarX.width = am4core.percent(70);

var cursor = new am4plugins_timeline.CurveCursor();
chart.cursor = cursor;
cursor.xAxis = dateAxis;
cursor.yAxis = categoryAxis;
cursor.lineY.disabled = true;
cursor.lineX.strokeDasharray = "1,4";
cursor.lineX.strokeOpacity = 1;

dateAxis.renderer.tooltipLocation2 = 0;
categoryAxis.cursorTooltipEnabled = false;

}); // end am4core.ready()





</script>

<main>
  <!-- <div class="column grey is-half is-offset-one-quarter"><p class="has-text-centered">W{week}</p></div> -->
<nav class="navbar is-light" role="navigation" aria-label="main navigation" id="main-navbar">

  <div class="navbar-item" id="context-week">
    <span class="navbar-item is-small mdi mdi-arrow-left" id="arrow"></span>
    <div class="center">
      <div>
        <p class="heading is-size-6" id="fy">FY{fy}</p>
        <p class="title" id="week">W{week}</p>
      </div>
    </div>
    <span class="navbar-item is-small mdi mdi-arrow-right" id="arrow"></span>
  </div>
  

  <div class="navbar-end">
      <div>
        <p class="heading is-size-6">{date}</p>
      </div>
  </div>
</nav>

<div id="chartdiv"></div>

</main>


<style>

#chartdiv {
  width: 100%;
  height: 600px;
}

.demo-theme-dark .demo-background {
  background: #000;
}

main{
  font-family: 'Lato', sans-serif;
}

#main-navbar{
  padding-right: 3%;
}

#fy{
  margin-bottom: -1px;
}

#week{
  margin-bottom: 5px;
}

</style>