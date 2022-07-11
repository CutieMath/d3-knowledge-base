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
- The instructor followed a very systematic approach first it select the root node then select all the nodes under the root node but not any node only the ones he is interested
  in creating in the final step this whole process might be call the update step. This is a good approach to avoid bugs if the nodes does or doesn't exist it won't make much difference.
  The update is the intersection of data with elements and elements with content correlated with the data. The exit is when there is not content correlated with data and the enter
  is just data but new within the existing set of elements.
- Call the method exit() to get all the elements within that set. Then apply anything like attr, remove, style etc
- Call the method enter() to get all the elements within that set. Then apply anything like attr, remove, style etc
- Call the method merge() from the object you get in the update step and pass the set you want to merge in the merge method
- update = d3.select('.chart') .selectAll('div') .data(scores, function (d) { return d ? d.name : this.innerText; }) .style('color', 'blue')
- enter = update.enter() .append('div') .text(function (d) { return d.name; }) .style('color', 'green')
- update.exit().remove()
- update.merge(enter) .style('width', d => d.score + 'px')

svg-output
svg-graphics-and-text
basic-interactivity
selection-call
margin-convention
chart-axes
responsive-viewbox
column-chart
scatter-plot
line-chart
area-chart
debug-devtools
animated-transitions
reusable-transitions
animate-update-pattern
animate-axis-updates
