---
title: Bar Chart
anchor: bar-chart
---

### Introduction
A bar chart is a way of showing data as bars.

It is sometimes used to show trend data, and the comparison of multiple data sets side by side.

<div class="canvas-holder">
	<canvas width="250" height="125"></canvas>
</div>

### Example usage
```javascript
var myBarChart = new Chart(ctx,{
	type: 'bar',
	data: data,
	options: options
});
```

### Data structure

```javascript
var data = {
	labels: ["January", "February", "March", "April", "May", "June", "July"],
	datasets: [
		{
			label: "My First dataset",

			// The properties below allow an array to be specified to change the value of the item at the given index
			// String  or array - the bar color
			backgroundColor: "rgba(220,220,220,0.2)",

			// String or array - bar stroke color
			borderColor: "rgba(220,220,220,1)",

			// Number or array - bar border width
			borderWidth: 1,

			// String or array - fill color when hovered
			hoverBackgroundColor: "rgba(220,220,220,0.2)",

			// String or array - border color when hovered
			hoverBorderColor: "rgba(220,220,220,1)",

			// The actual data
			data: [65, 59, 80, 81, 56, 55, 40],

			// String - If specified, binds the dataset to a certain y-axis. If not specified, the first y-axis is used.
			yAxisID: "y-axis-1",
		},
		{
			label: "My Second dataset",
			backgroundColor: "rgba(220,220,220,0.2)",
			borderColor: "rgba(220,220,220,1)",
			borderWidth: 1,
			hoverBackgroundColor: "rgba(220,220,220,0.2)",
			hoverBorderColor: "rgba(220,220,220,1)",
			data: [28, 48, 40, 19, 86, 27, 90]
		}
	]
};
```
The bar chart has the a very similar data structure to the line chart, and has an array of datasets, each with colours and an array of data. Again, colours are in CSS format.
We have an array of labels too for display. In the example, we are showing the same data as the previous line chart example.

The label key on each dataset is optional, and can be used when generating a scale for the chart.

### Chart Options

These are the customisation options specific to Bar charts. These options are merged with the [global chart configuration options](#getting-started-global-chart-configuration), and form the options of the chart.

The default options for bar chart are defined in `Chart.defaults.Bar`.

Name | Type | Default | Description
--- |:---:| --- | ---
stacked | Boolean | false |
*hover*.mode | String | "label" | Label's hover mode. "label" is used since the x axis displays data by the index in the dataset.
scales | Array | - | -
*scales*.xAxes | Array |  | The bar chart officially supports only 1 x-axis but uses an array to keep the API consistent. Use a scatter chart if you need multiple x axes.
*Options for xAxes* | | |
type | String | "Category" | As defined in [Scales](#scales-category-scale).
display | Boolean | true | If true, show the scale.
position | String | "bottom" | Position of the scale. Options are "top" and "bottom" for dataset scales.
id | String | "x-axis-1" | Id of the axis so that data can bind to it
categoryPercentage | Number | 0.8 | Percent (0-1) of the available width (the space between the gridlines for small datasets) for each data-point to use for the bars. [Read More](#bar-chart-barpercentage-vs-categorypercentage)
barPercentage | Number | 0.9 | Percent (0-1) of the available width each bar should be within the category percentage. 1.0 will take the whole category width and put the bars right next to each other. [Read More](#bar-chart-barpercentage-vs-categorypercentage)
gridLines | Array |  [See Scales](#scales) |
*gridLines*.offsetGridLines | Boolean | true | If true, the bars for a particular data point fall between the grid lines. If false, the grid line will go right down the middle of the bars.
scaleLabel | Array | [See Scales](#scales) |
ticks | Array |  [See Scales](#scales) |
| | |
*scales*.yAxes | Array | `[{ type: "linear" }]` |
*Options for xAxes* | | |
type | String | "linear" | As defined in [Scales](#scales-linear-scale).
display | Boolean | true | If true, show the scale.
position | String | "left" | Position of the scale. Options are "left" and "right" for dataset scales.
id | String | "y-axis-1" | Id of the axis so that data can bind to it.
gridLines | Array |  [See Scales](#scales) |
scaleLabel | Array | [See Scales](#scales) |
ticks | Array |  [See Scales](#scales) |

You can override these for your `Chart` instance by passing a second argument into the `Bar` method as an object with the keys you want to override.

For example, we could have a bar chart without a stroke on each bar by doing the following:

```javascript
new Chart(ctx,{
	type:"bar",
	data: data,
	options: {
		scales: {
				xAxes: [{
						stacked: true,
				}],
				yAxes: [{
						stacked: true
				}]
			}
		}
	}
});
// This will create a chart with all of the default options, merged from the global config,
//  and the Bar chart defaults but this particular instance will have `stacked` set to true
// for both x and y axes.
```

We can also change these defaults values for each Bar type that is created, this object is available at `Chart.defaults.Bar`.

#### barPercentage vs categoryPercentage

The following shows the relationship between the bar percentage option and the category percentage option.

```text
// categoryPercentage: 1.0
// barPercentage: 1.0
Bar:          | 1.0 | 1.0 |
Category: 	|    1.0    |   
Sample:	   |===========|

// categoryPercentage: 1.0
// barPercentage: 0.5
Bar:             |.5|  |.5|
Category: 	|      1.0     |   
Sample:	   |==============|

// categoryPercentage: 0.5
// barPercentage: 1.0
Bar:              |1.||1.|
Category:     	|  .5  |   
Sample:	   |==============|
```
### Prototype methods

#### .getBarsAtEvent( event )

Calling `getBarsAtEvent(event)` on your Chart instance passing an argument of an event, or jQuery event, will return the bar elements that are at that the same position of that event.

```javascript
canvas.onclick = function(evt){
	var activeBars = myBarChart.getBarsAtEvent(evt);
	// => activeBars is an array of bars on the canvas that are at the same position as the click event.
};
```

This functionality may be useful for implementing DOM based tooltips, or triggering custom behaviour in your application.

#### .update( )

Calling `update()` on your Chart instance will re-render the chart with any updated values, allowing you to edit the value of multiple existing points, then render those in one animated render loop.

```javascript
myBarChart.datasets[0].bars[2].value = 50;
// Would update the first dataset's value of 'March' to be 50
myBarChart.update();
// Calling update now animates the position of March from 90 to 50.
```

#### .addData( valuesArray, label )

Calling `addData(valuesArray, label)` on your Chart instance passing an array of values for each dataset, along with a label for those bars.

```javascript
// The values array passed into addData should be one for each dataset in the chart
myBarChart.addData([40, 60], "August");
// The new data will now animate at the end of the chart.
```

#### .removeData( )

Calling `removeData()` on your Chart instance will remove the first value for all datasets on the chart.

```javascript
myBarChart.removeData();
// The chart will now animate and remove the first bar
```