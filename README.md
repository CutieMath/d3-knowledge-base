# D3-knowledge-base

A repository to keep my learning experience of D3.js

# Install & Configure (01-install-and-configure)

- Download browser-sync with npm install browser-sync
- Create folder with the name of your chose and run npm init
- Create index.html and use emmet in your editor of choice to generate html5
- Create src directory and add a js file with the name of your choice and in your index.html add the
  script pointing to the path of that file.
- Add in the scripts object: "start": "browser-sync start -s -f index.html src --no-ui --no-notify"
- For better user experience install D3.js with npm to receive suggestion code from the editor

# Linear Scales (02-linear-scales)

- `d3.scaleLinear().domain([xs,xe]).range([ys,ye]).clamp(true)` the postfix i and e in x and y stand
  for start and end. The clamp is equal to true to follow strictly the range values min/ys and
  max/ye. This is equivalent to a mathematical function f(x) = y continuous and linear. The invert
  method is just f(y) = x

# Time Scales (03-time-scales)

- `d3.scaleTime() .domain([startmilliepoch, endmilliepoch]) .range([ys, ye]).` `Date()` object can
  be also used instead of a long data type

# Quantize Scales (04-quantize-scales)

- `d3.scaleQuantize() .domain([xs, xe]) .range([w0, w1, w2, ... , wn])`. The quantize scale can be
  visualise in a pie chart with all the elements with equal proportions. the number of elements in
  the range divides the domain interval into equal proportions and within every portion there will
  be a range of values that are map to the range set of elements AKA bijective

# Ordinal Scales (05-ordinal-scale)

```
 d3.scaleOrdinal().domain(['a', 'b', 'c']) .range(['x', 'y', 'z']).

```

- Also a bijective map e.x f(a) = x, f(b) = y, etc

# Loading Data (06-loading-data)

- In this section we saw D3 function such as

  ```
  d3.json(path, function()), d3.csv(path, function()), d3.json(path, function())
  ```

  There is more formats that were not covered within the course.
  We also saw d3.extend(dataArray) which returns the min and max of the data.
  `d3.set(dataArray, function())` returns all elements without duplicates and can be retrieve with
  `d3.set(dataArray, function()).values()`

# Select Dom Elements (07-select-dom-elements)

- One of the most ubiquitous functions in d3.js found everywhere the d3.select(element). The element
  inside the select function can be specified as a string, as an object coming from the document
  object provided by browsers such as document.links or can be also a CSS query such
  as 'a:nth-child(2)'. After the element has been selected then you can manipulated by appending
  more elements retrieving attributes, values, other nodes and more.
  The selectAll does something similar but instead retrieves an array matching the query.

# Modify Dom Elements (08-modify-dom-elements)

- In this part with use select and the apply modifications like adding attribute with the
  att('attribute', 'value') function.
  Adding classes in the classed function classed('class',booleanValue)
  very useful to change style dynamically or css attributes to the element. Also we saw the
  html('HTML text') meaning the elements inside the text will be parsed to HTML. In addition we saw
  the Text element which adds text but don't get parsed to anything

  ```
  d3.selectAll('a:nth-child(2)')
     .attr('href', 'http://google.com')
     .classed('red', true)
     .html('Inventory <b>SALE</b>');
  ```

# Create Dom Elements (09-create-dom-elements)

- In here we used the select and append function to add another element inside the selected one.
  `d3.select('.className').append('div')`

# Data Joins (10-data-joins)

- One of the most important modules since here we start combining select and data function. It is
  important to remember how to use it when we fetch data to display with D3.
- The procedure start with the following
- selectAll(elementXYZ) -> data(data, callback) - Use the callback to check if members of the data
  set match with HTML elements.
  Data that match with elements there is not need to retrieve as it is already retrieved by default.
  Elements that doesn't match with data can be retrieve by using the exit() function.
  Data that doesn't match with elements can be retrieve by using the enter() function.
  To merge the data that match with elements and data that doesn't use the merge function

  ```
  update = d3.select('.chart')
    .selectAll('div')
    .data(scores, function (d) { return d ? d.name : this.innerText; })
    .style('color', 'blue')
  ```

- update.exit().remove()
- enter = update.enter() .append('elementXYZ') .text(function (d) { return d.name; }) .style('color', 'green')
- update.merge(enter) .style('width', d => d.score + 'px')  
  
```
Yuxin:  
- Enter selection: DOM element not exist, data exist
- Exit selection: DOM element exist, data not exist
- Update selection: Have both
- Refers to: https://bost.ocks.org/mike/join/
```

# Svg Output (11-svg-output)

- Same as the previous module with the difference now of svg rect instead of div as the selection.
  There is also one change in the position of the rect in order to render and svg component the x,y
  position must be specified

  ```
  selectAll('rect').data(data, callback).enter().append('rect').attr('width', ( \_, i )=> i \* 33)
  ```

  Remember the i for index

# Svg Graphics & Text (12-svg-graphics-and-text)

- In this module we use the svg element 'g' for graphics. This time instead of specifying the x,y
  coordinate attributes we use the attr transform and call the function translate(x,y) the g
  component will contain the svg sub elements.

  ```
  var g = selectAll('g')
     .data(data)
     .enter()
     .append('g')
     .attr('transform', (d, i) => 'translate(0, ' + i \* 33 + ')')
  ```

  After we map the data with the g element then we can use other element inheriting the position of
  the g element.

  ```
  g.append('rect')
  g.append('text')
  ```

  Result

  ```
  <g><rect/><text/></g>
  ```

# Basic Interactivity (13-basic-interactivity)

- In this module we use the chain function on(event, callback) look on developer.mozilla.org for all
  the events. Remember to remove the on prefix in the first parameter of the on function.
  https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Events
- Another important point is that in the callback you can use the 'this' keyword to refer to the
  current element with the iteration. For example
  ```
  bar.append('rect').on('mouseover', () => d3.select(this).attr('color', 'blue'))
  ```

# Selection Call (14-selection-call)

- In this module we use the call function from the selection API for better maintenance and
  readability. It works by passing a function in the first argument and then the arguments for the
  function that has been passed. The function use as the first argument will have this signature
  ```
  d3.select(elemnt).call(function(selection, param1, param2,..., paramN),param1, param2,..., paramN)
  ```
  notes that you don't need to pass the selection; it is passed automatically.

# Margin Convention (15-margin-convention)

- The margin selection follows the convention of 3 variables first the margin object with properties
  top,right,bottom,left, second width and third height. Subtract in width the margins left and right
  with the intended width. Do the same for bottom and top for height. Then the root svg will have
  attributes height and width by adding back the subtracted values.

  ```
  var margin = { top: 10, right: 20, bottom: 25, left: 25 };
  var width = 425 - margin.left - margin.right;
  var height = 625 - margin.top - margin.bottom;

  var svg = d3.select('.chart')
     .append('svg')
     .attr('width', width + margin.left + margin.right)
     .attr('height', height + margin.top + margin.bottom)
     .append('g')
     .attr('transform', `translate(${margin.left}, ${margin.top})`);
  ```

- Why this works? Because the margin left and top work as the x,y coordinate for the g svg element
  and the width and height used in subsequent elements will be used as the margin for the right and
  bottom margin for example

  ```
  svg.append('rect').attr('width', width).attr('height', height)
  ```

  The rect will have start the rendering from the x,y coordinate specified from left,top margins and
  end the rendering on the x,y coordinates specified in the height and width

# Chart Axes (16-chart-axes)

- The chart axes are used in combination with the d3 scale API for example for the y axis use linear
  scale but remember that the y coordinate is from top to bottom and not the other way, so in the
  range you need to specify height, 0 instead of 0,height use axisLeft and pass the linear scale
  for the y axis and then use the call function from the svg root object created with d3
  use axisBottom and pass the scale for the x axis and then use the call function from the svg root
  object created with d3 for the xaxis remember that it will render from the x,y initial coordinate
  of the root svg element so you will need to translate the xaxis coordinate manually be using the
  transform attribute and translate function

  ```
  var yScale = d3.scaleLinear().domain([0, 100]).range([height, 0]);
  var yAxis = d3.axisLeft(yScale);
  svg.call(yAxis);

  var xScale = d3.scaleLinear().domain([0, 100]).range([0, width]);
  var xAxis = d3.axisBottom(xScale).tickSize(10).tickPadding(5);
  svg.append("g").attr("transform", `translate(0, ${height})`).call(xAxis);
  ```

- You can use the ticks function for better visualization

# Responsive Viewbox (17-responsive-viewbox)

- If the root svg element targeted for re-sizing already has the attribute viewBox then you can
  create a function as use call(function) on the root svg element.
- The function consist first in getting the parent node of the root svg (normally it is a div)
- Then extract the define width and height from svg to calculate the aspect ratio.
- call resize function
- select window element and onresize event pass the resize function as parameter
- Create resize function inside the parent function; it consist of getting the width from the
  parent node of the svg which doesn't have a fixed width the assign that new width to the svg
  and compute the height by routing the division of the new width and aspect ratio then assign
  that to the svg height.
- Note the
- Preserve Aspect Ratio was used to keep the alignment of the SVG viewBox in this case it will
  align from the center see this link for a better explanation
  https://viewbox.club/tips/07.SVG_preserveAspectRatio.html

  ```
  function responsivefy(svg) {
     var container = d3.select(svg.node().parentNode),
     aspect = parseInt(svg.style("width")) / parseInt(svg.style("height"));
     resize();
     d3.select(window).on("resize." + 1, resize);
     function resize() {
        var targetWidth = parseInt(container.style("width"));
        svg.attr("width", targetWidth);
        svg.attr("height", Math.round(targetWidth / aspect));
     }
  }
  ```

# Column Chart (18-column-chart)

- In this lecture we use the column chart. As we already learned from chart-axis module this time
  we need to display the data on the coordinates. Here we use the scale band as follow:
  create a scale band as any other scales it is optional if you want padding with values from 0
  to 1. The we use the RECT SVG element and it is rendered by first specifying the x and y
  coordinates using the xScale and yScale function respectively and passing the corresponding
  arguments. The use the xScale bandwidth for the width and height the actual height of the area of
  the SVG parent node of the axis subtracted with the result of the yScale function. Why this
  works?

  Remember the origin of the y coordinate is not in the center or bottom but on the top and starts
  from the top so the y axis starts from the top and the yScale function will output a value that
  is count from the top as follows

  ```
  yAxis  0   yScale -> starts count 0
         .                          |
         .                          |
         .              ends count (.) From end count subtract yAxis - yScale (.)
         .                                                                     .
         .                                                                     .
         .                                                                     .
         .                                                                     .
         .                                                                     .
         .                                                                     .

  ```

  Hence the last attribute .attr("height", (d) => height - yScale(d.score)). In here we subtract the
  height by the yScale function result to obtain the height of the bar and place it in the x,y
  coordinate.

  ```
  svg
     .selectAll("rect")
     .data(data)
     .enter()
     .append("rect")
     .attr("x", (d) => xScale(d.subject))
     .attr("y", (d) => yScale(d.score))
     .attr("width", (\_) => xScale.bandwidth())
     .attr("height", (d) => height - yScale(d.score));
  ```

  In here we also saw useful text style and transformation such as

  ```
  d3.select('div').append("text")
     .style("text-anchor", "end")
     .attr("transform", "rotate(-45)");
  ```

# Scatter Plot (19-scatter-plot)

- In this module we use the nice() function so that the domain start and ends are rounded to the
  nearest value.
- Here we use what we learned in Chart Axes module with the addition of rendering circles in the
  coordinate system. For that we use the scaleSqrt() function so the difference of the radius are
  not disproportionately high.
- To create a chart like this from scratch start first with the chart axis then create a G
  SVG element and place it with the xScale and yScale function then using the transform attribute
  and translate function. Then use the G element object created from d3 and render circles as well
  as text using without the need to specify the x,y coordinate as it was already done in the
  transform attribute. For the radius use the scaleSqrt() function.

  ```
  d3.json("./data.json", function (err, data) {
    var yScale = d3
      .scaleLinear()
      .domain(d3.extent(data, (d) => d.expectancy))
      .range([height, 0])
      .nice();

    var yAxis = d3.axisLeft(yScale);

    svg.call(yAxis);

    var xScale = d3
      .scaleLinear()
      .domain(d3.extent(data, (d) => d.cost))
      .range([0, width])
      .nice();

    var xAxis = d3.axisBottom(xScale).ticks(5);
    svg.append("g").attr("transform", `translate(0, ${height})`).call(xAxis);

    var rScale = d3
      .scaleSqrt()
      .domain([0, d3.max(data, (d) => d.population)])
      .range([0, 40]);

    var circles = svg
      .selectAll(".ball")
      .data(data)
      .enter()
      .append("g")
      .attr("class", "ball")
      .attr("transform", (d) => {
        return `translate(${xScale(d.cost)}, ${yScale(d.expectancy)})`;
      });

    circles
      .append("circle")
      .attr("cx", 0)
      .attr("cy", 0)
      .attr("r", (d) => rScale(d.population))
      .style("fill-opacity", 0.5)
      .style("fill", "steelblue");

    circles
      .append("text")
      .style("text-anchor", "middle")
      .style("fill", "black")
      .attr("y", 4)
      .text((d) => d.code);
  });
  ```

# Line Chart (20-line-chart)

- In line chart we still used what we learned in Chart Axis module. In addition we need to render a
  line in the x,y coordinate. In order to do that we need to create a line that is connected through
  specific values in the coordinate system. d3.line is a function that receives the data and it can
  return all the set of x and y values in a format for the PATH SVG element. In is important to
  understand how the PATH SVG element works (see the link below) but for simplicity think about it
  as an element that only need a d attribute such as <path d="M23,2 L34,3"/> the M and L are
  commands for the path and the numbers are coordinates. The path will be connected and that it is
  how you get a line chart.

  ```

  // Here we create the line to obtain the set of all coordinates by passing the values line(values)
  // The result will be something like "M2,4L12,14L22,4" which will be use for the path element
  var line = d3
    .line()
    .x((d) => xScale(d.date))
    .y((d) => yScale(d.close))
    // This smoothes the line path giving a curve Look and Feel
    .curve(d3.curveCatmullRom.alpha(0.5));

  // Here we create the path svg element and in the d attribute we pass the line function with the
  // data values. a string will be returned like "M2,4L12,14L22,4" this is just random and might no
  // be necessarily the actual string
  svg
    .selectAll(".line")
    .data(data)
    .enter()
    .append("path")
    .attr("class", "line")
    .attr("d", (d) => line(d.values))
    //Here we can render the line with different colors as the data is an array of 2 elements
    .style("stroke", (d, i) => ["#FF9900", "#3369E8"][i])
    .style("stroke-width", 2)
    .style("fill", "none");

  ```

# Area Chart (21-area-chart)

- Similar as line chart this time with the difference of using the area function instead of line.
  The area function works similar to line with the addition of a new fluent method (y0) and
  renaming the y() method for y1(). The y0 function tells the area how to fill the path imagine
  a holding a rod at their end with your hands stretched so that the rod is held horizontally by
  moving it upwards the points that were above are filled with smaller area and the points that were
  below are filled with bigger area; the contrary happens when the rod is moved downwards.

  ```
  // similar to line with the below difference
  var area = d3.area()
    ...
    .y0(yScale(yScale.domain()[0]))
    .y1(d => yScale(d.close))
    ...

  When rendering the path you can use the one below to differenciate areas by different colors
  .style('fill', (d, i) => ['#FF9900', '#3369E8'][i])
  ```

# Debug devtools (22-debug-devtools)

- The `$0` in the console when you select an element in the Elements tab the console will display
  that Dom element.
- The `$0.__data__` will output in the console the data that is being render by that element
- Modify the element from the console if you have it available by using `d3.select($0)`
- In the Styles tab you can change properties to see it render in the browser as you edit the styles
- In this module the filter function such as `d3.selectAll(element).filter(callback)` was used to
  filter elements satisfying the callback condition.

# Animated transitions (23-animated-transitions)

- Really interesting module. Transitions in d3 works in a sequential way when you modify the
  attributes for example when setting the width with a transition and then the height with another
  transition; it will be animated based on the sequential attribute changes.

  ```
  // Note here the three transition happen sequentially starting first with width then  height and
  then color. You can change it by declaring first in the code the height then width and color.
  d3.select('#block')
    .transition()
      .duration(600)
      .delay(750)
      .ease(d3.easeBounceOut)
      .style('width', '400px')
    .transition()
      .duration(600)
      .ease(d3.easeBounceOut)
      .style('height', '600px')
    .transition()
      .duration(2000)
      .ease(d3.easeQuadOut)
      .style('background-color', 'purple')
  ```

# Reusable transitions (24-reusable-transitions)

- In this module we saw different ways of reusing transitions such as getting a transition object
  by calling `t = d3.transition().duration(600).delay(600)` and assign it to a variable then to
  animate an element attribute change pass the transition object to transition method such as  
  `d3.select(element).transition(t).style('width','400px')` REMEMBER not to get the transition
  object globally since the transition is called from the moment the browser renders; call it
  instead within a function scope.

  ```
  function go () {
    var t = d3.transition()
      .delay(1000)
      .duration(1000);

    d3.selectAll('.block')
      .transition(t)
      .style('width', '400px');
  }
  ```

- The other way of reusing transitions is by using the call method from the transition API, similar
  to the call method from the selection API. This time the call method will pass a transition object
  instead of an element object.

  ```
   function configure (t, delay, duration) {
     return t.delay(delay).duration(duration);
   }

   function goNow () {
     d3.selectAll('.block')
       .transition()
       .call(configure, 1000, 1000)
       .style('height', '200px');
   }
  ```

# Animate update pattern (25-animate-update-pattern)

- Here we use what we learn in data join, transition (first option for reusing transitions). The
  code might look a bit complicated but conceptually it is really simple. Remember that transition
  happen as you declare it sequentially in the code. So if you interact with the code you'll see
  that first the bar shrinks from top to bottom, the rest of the bars are updated and a new bar
  expands from bottom to top. As you can see in the code below the transitions are use first in the
  exit function (elements that doesn't match data) then another transition in the update state (data
  and elements that match), and then another transition in the enter function (data that doesn't
  match with elements)

  ```
  function render(subject = "math") {
    var t = d3.transition().duration(1000);

    var update = svg.selectAll("rect").data(
      data.filter((d) => d[subject]),
      (d) => d.name
    );

    update.exit().transition(t).attr("y", height).attr("height", 0).remove();

    update
      .transition(t)
      .delay(1000)
      .attr("y", (d) => yScale(d[subject]))
      .attr("height", (d) => height - yScale(d[subject]));

    update
      .enter()
      .append("rect")
      .attr("y", height)
      .attr("height", 0)
      .attr("x", (d) => xScale(d.name))
      .attr("width", (d) => xScale.bandwidth())
      .transition(t)
      .delay(update.exit().size() ? 2000 : 0)
      .attr("y", (d) => yScale(d[subject]))
      .attr("height", (d) => height - yScale(d[subject]));
  }

  ```

# Animate axis updates (26-animate-axis-updates)

- In this module we used pretty much the same logic from the previous one with the only difference
  that this time we also animate the Y axis. In the code below these were the only additions. As you
  can see we now get a variable yAxis which is just a G SVG element that acts as a container for the
  yAxis. This will be used in the render function to call the transition by updating first the
  domain and range with the new values using the call method.

  ```
  ...
  var yAxis = svg
    .append('g')
    .call(d3.axisLeft(yScale));
  ...

  function render(subject = "math") {
  ...
  yScale.domain([0, d3.max(data, d => d[subject])]);
  yAxis
    .transition(t)
    .delay(1000)
    .call(d3.axisLeft(yScale));
  ...
  ```

# References

- Ben Clinkinbeard, Build Interactive JavaScript Charts with D3 v4, 2016
  https://egghead.io/courses/build-interactive-javascript-charts-with-d3-v4

- Ben Clinkinbeard, Github Repo
  https://github.com/bclinkinbeard/egghead-d3v4
