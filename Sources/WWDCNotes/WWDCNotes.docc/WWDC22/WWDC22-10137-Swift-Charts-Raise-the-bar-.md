# Swift Charts: Raise the bar 

Dive deep into data visualizations: Learn how Swift Charts and SwiftUI can help your apps represent complex datasets through a wide variety of chart options. We’ll show you how to plot different kinds of data and compose marks to create more elaborate charts. We’ll also take you through Swift Charts’ extensive chart customization API to help you match the style of your charts to your app.

@Metadata {
   @TitleHeading("WWDC22")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc22/10137", purpose: link, label: "Watch Video (21 min)")

   @Contributors {
      @GitHubUser(Jeehut)
      @GitHubUser(multitudes)
   }
}


## Intro

This session is about how to build great data visualizations with Swift Charts,

```swift
Chart (data, id: .name) {
	BarMark(
		x: .value ("Sales", $0. sales),
		y: .value ("Name", $0. name)
	)
}
```
![swiftui charts][charts1]

[charts1]: WWDC22-10137-charts1  

Which can be easily customisable.

![swiftui charts][charts2]

[charts2]: WWDC22-10137-charts2  

- declarative syntax, like SwiftUI
- create charts by composition

Some examples of charts on the Apple platform:

![swiftui charts][charts3]

[charts3]: WWDC22-10137-charts3  

- Problem space of chart libraries
  - Data Visualization
  - Communicate Data
  - Accessible
  - Localization
  - Dark Mode
  - Layout
  - Dynamic Type
  - Device Screen Sizes
  - Multi-Platform
  - Animation

# Marks and composition

- a `Mark` is a graphical element that represents data

![swiftui charts][charts4]

[charts4]: WWDC22-10137-charts4

![swiftui charts][charts5]

[charts5]: WWDC22-10137-charts5

- `Chart` is a SwiftUI view used as the wrapper for charts

```swift
struct TopStyle: View {
	var body: some View {
		GroupBox ("Most Sold Style") {
			Text ("Cachapa")
			Chart {
				// TODO: Add contents in the chart.
			}
		}
	}
}
```

![swiftui charts][charts5]

[charts5]: WWDC22-10137-charts5

- This is a chart  with one BarMark
```swift
Chart {
	BarMark(
		x: .value("Sales", 916),
		y: .value ("Name", "Cachapa")
	)
}
```

![swiftui charts][charts6]

[charts6]: WWDC22-10137-charts6

- Provide multiple `BarMark` views to show multiple bars
- `.foregroundStyle` can be used to specify a color

```swift
let data = [
	(name: "Cachapa", sales: 916), 
	(name: "Injera", sales: 850), 
	(name: "Crêpe", sales: 802),
]

Chart(data, id: \.name) {
	BarMark(
		x: .value ("Sales", $0.sales),
		y: value ("Name", $0. name)
	)
	.foregroundStyle(Color ("barColor"))
}
```

![swiftui charts][charts7]

[charts7]: WWDC22-10137-charts7

- `.accessisibilityLabel` and `.accessibilityValue` can be used to customize the default VoiceOver value

```swift
Chart(data, id: \.name) {
	BarMark(
		x: .value ("Sales", $0.sales),
		y: value ("Name", $0. name)
	)
	.accessibilityLabel($0.name)
	.accessibilityValue("\($0.sales) sold")
}
```

- Data driven, e.g.:

```swift
let dailySales: [(day: Date, sales: Int)] = [
  (day: /* May 8th */, sales: 168),
  (day: /* May 9th */, sales: 117),
  (day: /* May 10th */, sales: 106),
  ...
]

Chart {
  ForEach(dailySales, id: \.day) {
    BarMark(
      x: .value("Day", $0.day, unit: .day),
      y: .value("Sales", $0.sales)
    )
  }
}
```

![swiftui charts][charts8]

[charts8]: WWDC22-10137-charts8

- switching chart types is easy, e.g. replace `BarChart` with `LineChart` (or `PointChart`) as in this example with two series of data.

```swift
let cupertinoData: [(weekday: Date, sales: Int)] = [...]
let sfData: [(weekday: Date, sales: Int)] = [...]

let seriesData = [
	(city: "Cupertino", data: cupertinoData), 
	(city: "San Francisco", data: sfData)
]

Chart {
	ForEach(seriesData, id: I.city) { series in
		ForEach(series.data, id: \.month) {
			LineMark(
				x: .value ("Weekday", $0.weekday, unit: .day),
				y: .value("Sales", $0.sales)
			)
		}
		.foregroundStyle(by: value("City", series .city))
		.symbol(by: value("City", series.city))
		.interpolationMethod(.catmullRom)
	}
}
```
![swiftui charts][charts9]

[charts9]: WWDC22-10137-charts9

- `x` parameter specifies the x-axis, `y` param the y-axis
- use `.foregroundStyle(by: .value("City", series.city)` for automatic coloring
- Use `.symbol(by: .value...)` for different shapes and for accessibility, in case the user is color blind
- Supports `.interpolationMethod(.catmullRom)` for smoothing out the curve
- Supports `.position` for grouping

```swift
	.position (by: .value("City", series.city))
```

![swiftui charts][charts10]

[charts10]: WWDC22-10137-charts10

## More types of marks

![swiftui charts][charts11]

[charts11]: WWDC22-10137-charts11

- `AreaMark` with `x:yStart:yEnd` to show a range of data

```swift
let data = [
	(month: /* Jul, 2021 */,
	dailyAverage: 127, 
	dailyMin: 95, 
	dailyMax: 194
	),
	...
]

Chart {
	ForEach(data, id: \.month) {
		AreaMark(
			x: .value ("Month", $0.month, unit: .month), 
			yStart: .value("Daily Min", $0.dailyMin), 
			yEnd: .value("Daily Max", $0.dailyMax)
		)
		.opacity (0.3)
		LineMark(
			x: value("Month", $0.month, unit: .month),
			y: .value ("Daily Average", $0 .dailyAverage)
		)
	}
}
```

![swiftui charts][charts12]

[charts12]: WWDC22-10137-charts12

- Also works with `BarMark` (to show range of data)
- `RectangleMark` shows distinct marks for mid instead of building a graph in `BarMark`

```swift
let data = [...]

Chart {
	ForEach(data, id: \.month) {
		BarMark(
			x: .value ("Month", $0.month, unit: .month), 
			yStart: .value("Daily Min", $0.dailyMin), 
			yEnd: .value("Daily Max", $0.dailyMax),
			charts13.jpg
		)
		.opacity (0.3)
		RectangleMark(
			x: value("Month", $0.month, unit: .month),
			y: .value ("Daily Average", $0 .dailyAverage),
			width: .ratio(0.6),
			height: 2
		)
	}
}
```

![swiftui charts][charts13]

[charts13]: WWDC22-10137-charts13

- `RuleMark(y:)` can be used alongside with `.annotation(position:)` for guiding lines

```swift
let data = [...]
let averageValue = 137

Chart {
	ForEach(data, id: \.month) {
		BarMark(...)
		RectangleMark(...)
	}
	.foregroundStyle(.gray.opacity(0.5))
	
	RuleMark(
		y: .value("Average", averageValue)
	)
	.lineStyle(StrokeStyle(lineWidth: 3))
	.annotation(position: •top, alignment: .leading) {
		Text ("Average: \(averageValue, format: .number)")
			.font(.headline)
			.foregroundStyle(.blue)
	}
}
```

![swiftui charts][charts14]

[charts14]: WWDC22-10137-charts14

Example of different ways to use and combine these basic marks. In order

- box plot  
- multi-series line chart  
- population pyramid  
- range plot  
- stream graph  
- multi-series scatter plot  
- heat map  
- a plot of a vector field

![swiftui charts][charts15]

[charts15]: WWDC22-10137-charts15


## Plotting data with mark properties

![swiftui charts][charts16]

[charts16]: WWDC22-10137-charts16

- Data Types
  - Quantitative (Int, Double, Decimal)
  - Nominal (String, Continent, ProductType)
  - Temporal (day: Date, time: Date)

- Quantitative Sales, Nominal Name

![swiftui charts][charts17]

[charts17]: WWDC22-10137-charts17

- Orientation of the bar depends on where the nominal data is

![swiftui charts][charts18]

[charts18]: WWDC22-10137-charts18

- Available data marks and properties:

![swiftui charts][charts19]

[charts19]: WWDC22-10137-charts19

- `scale` is available for all data types, e.g. `yScale`

```swift
func yScale(sales: Int) -> CGFloat {
	return CGFloat(sales) * someFactor + someOffset
}
```

![swiftui charts][charts20]

[charts20]: WWDC22-10137-charts20

- by default, the lib infers the scales automatically

![swiftui charts][charts21]

[charts21]: WWDC22-10137-charts21

- Use `.chartYScale(domain:)` modifier to have fixed range of values on the y axis

```swift
Chart {
	ForEach(seriesData, id: \city) { series in
		ForEach(series .weekdays, id: \.month) {
			LineMark(
				x: .value("Weekday", $0.weekday, unit: .day),
				y: value("Sales", $0.sales)
			)
		}
		.foregroundStyle(by: .value("City", series.city)) 
		.symbol(by: value("City", series .city))
	}
}
.chartYScale(domain: 0 ... 200)
```

![swiftui charts][charts22]

[charts22]: WWDC22-10137-charts22

- use the chartForegroundStyleScale modifier to change the colors of the lines on the chart 

```swift
Chart {
	ForEach(seriesData, id: \city) { series in
		ForEach(series .weekdays, id: \.month) {
			LineMark(
				x: .value("Weekday", $0.weekday, unit: .day),
				y: value("Sales", $0.sales)
			)
		}
		.foregroundStyle(by: .value("City", series.city)) 
		.symbol(by: value("City", series .city))
	}
}
.chartYScale(domain: 0 ... 200)
.chartForegroundStyleScale([
	"San Francisco" : .orange,
	"Cupertino": .pink
])
```

![swiftui charts][charts23]

[charts23]: WWDC22-10137-charts23

# Customizations

- All elements (axes, legends, plot area) are customizable

![swiftui charts][charts24]

[charts24]: WWDC22-10137-charts24

- `.chartXAxis {}` with `AxisMarks` inside can be used for custom axis

```swift
let data: [(month: Date, sales: Int)] = [...]

Chart(data, id: \.month) {
	BarMark(
		x: .value("Month", $0.month, unit: .month),
		y: .value("Sales", $0.sales)
	)
}
.chartXAxis {
	AxisMarks()  // this defaults to the standard config 
}
```

- Use `AxisGridLine`, `AxisTick` and `AxisValueLabel` with marks

```swift
let data: [(month: Date, sales: Int)] = [...]

Chart(data, id: \.month) {
	BarMark(
		x: .value("Month", $0.month, unit: .month),
		y: .value("Sales", $0.sales)
	)
}
.chartXAxis {
	AxisMarks (values: .stride(by: .month)) { value in
		AxisGridLine()
		AxisTick()
		AxisValueLabel(
			format: .dateTime.month(.narrow)
		)
	}
}
```

![swiftui charts][charts25]

[charts25]: WWDC22-10137-charts25

- also supports `if` kind of conditionals for dynamic axis and showing quarter data

```swift
let data: [(month: Date, sales: Int)] = [...]

Chart(data, id: \.month) {
	...
}
.chartXAxis {
	AxisMarks (values: .stride(by: .month)) { value in
		if value.as(Date.self)!.isFirstMonthOfQuarter {
			AxisGridLine().foregroundStyle(.black)
			AxisTick().foregroundStyle(.black)
			AxisValueLabel(
				format: .dateTime.year().quarter()
			)
		} else {
			AxisGridLine ( )
		}
	}
}
```

![swiftui charts][charts26]

[charts26]: WWDC22-10137-charts26

- `AxisMarks` accepts a `position` param, e.g. `.leading`
- `AxisMarks` accepts `preset` param, e.g. `.extended` to align visually with rest of interface

```swift
let data: [(month: Date, sales: Int)] = [...]

Chart(data, id: \.month) {
	...
}
.chartYAxis {
	AxisMarks(preset: .extended, position: .leading)
```

- hide axis via `.chartXAxis(.hidden)` for example

```swift
Chart {
	...
}
.chartXAxis(.hidden)
.chartYAxis (.hidden)
```

- hide legend via `.chartLegend(.hidden)`
- `.chartPlotStyle {}` to customize plot area
  - e.g., `plotArea.frame(height: 60 * numberOfCategories)`
  - or `plotArea.background(.pink.opacity(0.2))`, any modifiers for views available
```swift
Chart {
	...
}
.chartPlotStyle { plotArea in 
	plotArea.background(.pink.opacity(0.2))
}
```

![swiftui charts][charts27]

[charts27]: WWDC22-10137-charts27

- `ChartProxy` can be used to access the `position(forX:)` or `value(atX:)`

```swift
let proxy: ChartProxy

proxy.position(forX: 123.0) // get the X position for value 123.0.
proxy.value(atX: 100) // get the data value at X position 100pt.
```

- allows to coordinate other views with chart, e.g. select an interval in the chart with a slider

- `.chartOverlay` modifier provides a `ChartProxy` in the content

```swift
Chart {
	...
}
.chartOverlay { proxy in
	// Define the overlay view as a function
	// of the chart proxy.
}
.chartBackground { proxy in
	// Define the background view as a function
	// of the chart proxy.
}
```
- example

```swift
struct InteractiveBrushingChart: View {
  @State var range: (Date, Date)? = nil

  var body: some View {
    Chart { ... }
    .chartOverlay { proxy in
      GeometryReader { nthGeoItem in
        Rectangle().fill(.clear).contentShape(Rectangle())
          .gesture(DragGesture()
            .onChanged { value in
              // Find the x-coordinates in the chart’s plot area.
              let xStart = value.startLocation.x - nthGeoItem[proxy.plotAreaFrame].origin.x
              let xCurrent = value.location.x - nthGeoItem[proxy.plotAreaFrame].origin.x
              // Find the date values at the x-coordinates.
              if let dateStart: Date = proxy.value(atX: xStart),
                 let dateCurrent: Date = proxy.value(atX: xCurrent) {
                range = (dateStart, dateCurrent)
              }
            }
            .onEnded { _ in range = nil } // Clear the state on gesture end.
          )
      }
    }
  }

  // ...
}
```

![swiftui charts][charts28]

[charts28]: WWDC22-10137-charts28

the code will look like this

```swift
@State var range: (Date, Date)? = nil

Chart {
...
	if let range = range {
		RectangleMark(
			xStart: .value("Range Start", range.0), 
			xEnd: .value("Range End", range.1)
		)
		.foregroundStyle(.gray.opacity(0.2))
	}
}
.chartOverlay { proxy in
   ...
}
```

- the proxy allows to store data in state and drive the chart rendering with that data (e.g. for a hover and show data effect)

![swiftui charts][charts29]

[charts29]: WWDC22-10137-charts29



# Resources

[Creating a chart using Swift Charts](https://developer.apple.com/documentation/Charts/Creating-a-chart-using-Swift-Charts)  
[Have a question? Ask with tag wwdc2022-10137](https://developer.apple.com/forums/create/question?&tag1=386030&tag2=423030)  
[Search the forums for tag wwdc2022-10137](https://developer.apple.com/forums/tags/wwdc2022-10137)  
[Swift Charts](https://developer.apple.com/documentation/Charts)  
[Visualizing your app’s data](https://developer.apple.com/documentation/charts/visualizing_your_app_s_data)

## Tech Talks
[What's new for enterprise developers](https://developer.apple.com/videos/play/tech-talks/110356)

## Related Videos

[Build a productivity app for Apple Watch - WWDC22](https://developer.apple.com/videos/play/wwdc2022/10133)  
[Design an effective chart - WWDC22](https://developer.apple.com/videos/play/wwdc2022/110340)  
[Design app experiences with charts - WWDC22](https://developer.apple.com/videos/play/wwdc2022/110342)  
[Hello Swift Charts - WWDC22](https://developer.apple.com/videos/play/wwdc2022/10136)  
[What's new in SwiftUI - WWDC22](https://developer.apple.com/videos/play/wwdc2022/10052)  
[WWDC22 Day 1 recap - WWDC22](https://developer.apple.com/videos/play/wwdc2022/110929)
