# d3-knowledge-base

A repository to keep my learning experience of d3

install-and-configure

- Download browser-sync with npm install browser-sync
- Create folder with the name of your chose and run npm init
- Create index.html and use emmet in your editor of choice to generate html5
- Create src directory and add a js file with the name of your choice and in your index.html add the script pointing to the path of that file.
- Add in the scripts object: "start": "browser-sync start -s -f index.html src --no-ui --no-notify"
- For better user experience install d3 with npm to receive suggestion code from the editor

linear-scales

- d3.scaleLinear().domain([xs,xe]).range([ys,ye]).clmap(true) the postfix i and e in x and y stand for start and end. The clamp is equal to true to follow strictly the range values min/ys and max/ye.
  This is equivalent to a mathematical function f(x) = y continous and linear. The invert method is just f(y) = x

time-scales

- d3.scaleTime() .domain([startmilliepoch, endmilliepoch]) .range([ys, ye]). Date() object can be also used instead of a long data type

quantize-scales

- d3.scaleQuantize() .domain([xs, xe]) .range([w0, w1, w2, ... , wn]). The quantize scale can be perfect for a pie chart the number of elements in the range divides the domain interval into equal
  proportions and within every portion there will be a range of values that are to the range set of elements AKA bijective

ordinal-scales

- d3.scaleOrdinal() .domain(['a', 'b', 'c']) .range(['x', 'y', 'z']). Also a bijective map e.x f(a) = x, f(b) = y, etc

loading-data

- In this section we saw d3 function such as d3.json(path, function()), d3.csv(path, function()), d3.json(path, function()) and there is more formats that were not covered within the course.
  We also saw d3.extend(dataArray) which returns the min and max of the data. d3.set(dataArray, function()) returns all elements without duplicates and can be retrieve with
  d3.set(dataArray, function()).values()

select-dom-elements

- One of the most obiquitios functions in d3 found everywhere the d3.select(element).
  The element inside the select funtion can be specified as a string, as an object coming from the document object provided by browsers such as document.links or can be also a css query such
  as 'a:nth-child(2)'. After the element has been selected then you can manipulated by appending more elements retrieving attributes, values, other nodes and more.
  The selectAll does something similar but instead retrieves an array matching the query.

modify-dom-elements

- In this part with use select and the apply modifications like adding attribute with the att('attribute', 'value') function. Adding classes in the classed function classed('class',booleanValue)
  very useful to change style dynamically or css attributes to the element. Also we saw the html('html text') meaning the elements inside the text will be parsed to html. In addition we saw
  the Text element which adds text but don't get parsed to anything
- d3.selectAll('a:nth-child(2)') .attr('href', 'http://google.com') .classed('red', true) .html('Inventory <b>SALE</b>');

create-dom-elements

- In here we used the select and append function to add another element inside the selected one. d3.select('.className').append('div')

data-joins

- One of the most important modules since here we start combining select and data function. It is important to remember how to use it when we fetch data to display with d3.
- The procedure start with the following
- selectAll(elementXYZ) -> data(data, callback) - Use the callback to check if members of the data set match with html elements.
  Data that match with elements there is not need to retrieve as it is already retrived by default.
  Elements that doesn't match with data can be retrieve by using the exit() function.
  Data that doesn't match with elements can be retrieve by using the enter() function.
  To merge the data that match with elements and data that doen't use the merge function
- update = d3.select('.chart') .selectAll('div') .data(scores, function (d) { return d ? d.name : this.innerText; }) .style('color', 'blue')
- update.exit().remove()
- enter = update.enter() .append('div') .text(function (d) { return d.name; }) .style('color', 'green')
- update.merge(enter) .style('width', d => d.score + 'px')

svg-output

- Same as the previous module with the difference now of svg rect instead of div as the selection. There is also one change in the position of the rect in order to render and svg component the x,y position must be specified
  selectAll('rect').data(data, callback).enter().append('rect').attr('width', ( \_, i )=> i \* 33)
  Remember the i for index

svg-graphics-and-text

- In this module we use the svg element 'g' for graphics. this time instead of specifying the x,y coordinate attributes we use the attr transform and call the function translate(x,y) the g componenet will contain the svg subelements.
  var g = selectAll('g').data(data).enter().append('g').attr('transform', (d, i) => 'translate(0, ' + i \* 33 + ')')
  After we map the data with the g element then we can use other element inheriting the position of the g element
  g.append('rect')
  g.append('text')
  <g><rect/><text/></g>

basic-interactivity

- In this module we use the chain function on(event, callback) look on developer.mozilla.org for all the events. remember to remove the on prefix in the first parameter of the on function. https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Events
- Another important point is that in the callback you can use the 'this' keyboard to refer to the current element with the iteration. for example
  bar.append('rect').on('mouseover', () => d3.select(this).attr('color', 'blue'))

selection-call

- In this module we use the call function from the selection api for better maintainance and readability. It works by passing a function in the first argument and then the arguments for the function that has been passed. The function use as the first argument will have this signature d3.select(element).call(function(selection, param1, param2, ..., paramN),param1, param2, ..., paramN ) notes that you don't need to pass the selection; it is passed automatically.

margin-convention

- The margin selection follows the convention of 3 variables first the margin object with properties top,right,bottom,left, second width and third height. Subtract in width the margins left and right with the intended width. Do the same for bottom and top for height. Then the root svg will have attributes height and width by adding back the subtracted values;

  var margin = { top: 10, right: 20, bottom: 25, left: 25 };
  var width = 425 - margin.left - margin.right;
  var height = 625 - margin.top - margin.bottom;

  var svg = d3.select('.chart')
  .append('svg')
  .attr('width', width + margin.left + margin.right)
  .attr('height', height + margin.top + margin.bottom)
  .append('g')
  .attr('transform', `translate(${margin.left}, ${margin.top})`);

- Why this works? becuase the margin left and top work as the x,y coordinate for the g svg element and the width and height used in subsequent elements will be used as the margin for the right and bottom margin for example

  svg.append('rect').attr('width', width).attr('height', height)

  The rect will have start the rendering from the x,y coordinate specified from left,top margins and end the rendering on the x,y coordinates specified in the height and width

chart-axes

- The chart axes are used in combination with the d3 scale api for example for the y axis use linear scale but remember that the y coordinate is from top to bottom and not the other way, so in the range you need to specify height, 0 instead of 0,height  
  use axisLeft and pass the linear scale for the y axis and then use the call function from the svg root object created with d3
  use axisBottom and pass the scale for the x axis and then use the call function from the svg root object created with d3 for the xaxis remember that it will render from the x,y initial coordinate of the root svg element so you will need to translate the xaxis coordinate manually be using the transform attribute and translate function

  var yScale = d3.scaleLinear().domain([0, 100]).range([height, 0]);
  var yAxis = d3.axisLeft(yScale);
  svg.call(yAxis);

  var xScale = d3.scaleLinear().domain([0, 100]).range([0, width]);
  var xAxis = d3.axisBottom(xScale).tickSize(10).tickPadding(5);
  svg.append("g").attr("transform", `translate(0, ${height})`).call(xAxis);

- You can use the ticks function for better visualisation

responsive-viewbox

- If the root svg element targeted for resizing already has the attribute viewbox then you can
  create a function as use call(function) on the root svg element.
- The function consist first in getting the parent node of the root svg (normally it is a div)
- Then extract the define width and height from svg to calculate the aspect ratio.
- call resize function
- select window element and onresize event pass the resize function as parameter
- Create resize function inside the parent function; it consist of getting the width from the
  parent node of the svg which doesn't have a fixed width the assign that new width to the svg
  and compute the height by rouding the division of the new width and aspect ratio then assign
  that to the svg height.
- Note the
- Preserve Aspect Ratio was used to keep the alignment of the svg viewbox in this case it will
  align from the center see this link for a better explenation
  https://viewbox.club/tips/07.SVG_preserveAspectRatio.html

  function responsivefy(svg) {
  // get container + svg aspect ratio
  var container = d3.select(svg.node().parentNode),
  aspect = parseInt(svg.style("width")) / parseInt(svg.style("height"));
  resize();
  // to register muktiple listeners for same event type,
  // you need to add namespace, i.e., 'click.foo'
  // necessary if you call invoke this function for multiple svgs
  // api docs: https://github.com/mbostock/d3/wiki/Selections#on
  d3.select(window).on("resize." + 1, resize);

  // get width of container and resize svg to fit it
  function resize() {
  var targetWidth = parseInt(container.style("width"));
  svg.attr("width", targetWidth);
  svg.attr("height", Math.round(targetWidth / aspect));
  }
  }

column-chart

- For the column chart we use the scale band
  scatter-plot
  line-chart
  area-chart
  debug-devtools
  animated-transitions
  reusable-transitions
  animate-update-pattern
  animate-axis-updates

//reate line chart
