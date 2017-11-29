---
layout: post
title: Marey Christmas
comments: true
---

<style>
svg {
  font: 10px sans-serif;
}

.axis path {
  display: none;
}

.axis line {
  stroke: #000;
  shape-rendering: crispEdges;
}

.station line {
  stroke: #ddd;
  stroke-dasharray: 1,1;
  shape-rendering: crispEdges;
}

.station text {
  text-anchor: end;
}

.train path {
  fill: none;
  stroke-width: 1.5px;
}

.train circle {
  stroke: #fff;
}

.train .C path { stroke: rgb(34,34,34); }
.train .C circle { fill: rgb(34,34,34); }

.train .S path { stroke: rgb(183,116,9); }
.train .S circle { fill: rgb(183,116,9); }
</style>


I *hate* Christmas-time scheduling. It's a pretty busy time of year and it
involves rushing around the country trying to see as many family members as
possible. Scheduling this in is really fiddly, as Sophie and I live about 70
miles away from various parts of our families. This means that we have to spend
a lot of time trying to figure out the best possible way for us to see as much
of our respective families as possible --- all while trying to maximise the
amount of time that *we* spend together as a couple as well. Another
complication is that Sophie works in the retail sector and only tends to get 3-4
days' holiday (as it turns out, retailers are rather stingy with their Christmas
holiday allowances!). Bundle all of these problems together and what you have is
a scheduling nightmare.

The biggest problem that I have when coming up with Christmas plans is
visualising the schedule in my head --- keeping track of which of us drives to
whose house and when (we usually drive down separately because I get a couple of
days' more holiday than Sophie). This got me thinking: is there a visualisation
that can help?

Enter Marey's train schedule diagram. [Etienne-Jules Marey][ejm] was a French
scientist, cinematographer, and general all-round brainbox in the late 19th
Century --- and one of his more well-known creations is a neat visualisation
method for train schedules. I first came across this image on the front cover
of Edward Tufte's famous dataviz masterpiece ["The Visual Display of
Quantitative Information"][tuf], but I'd also come across an [implementation in
D3 by Mike Bostock][d3v] for the metro in the San Francisco Bay Area. I thought
that this could probably be adapted to fit my needs quite nicely.

![Marey's train schedule](/images/marey.jpg)

So, here's a recap of the problem. Sophie and I have different amounts of
holiday. We have three different groups of families to see, and must spend *at
least* one evening with each group. With it being Christmas, we'd also like to
minimise the amount of driving (because we'd rather be spending that time with
family --- and let's face it --- we'd probably like to have a couple of glasses
of wine, too). We'd also like to spend as much time together as possible
because...  well... we're a couple, after all.

After an hour or two tinkering with D3, I modified Mike Bostock's original
version to something that looked a little like this (the black line is me, the
gold line is Sophie).

<div id="visualisation"></div>

Long story short, this neat diagram helped me visualise what we'd be doing a
little more clearly --- and subsequently allowed me to make a couple of tweaks
to our schedule that meant we'd be spending more time with our families (with
less travelling, too!).

And that's the story of how ~~the Grinch~~ Etienne-Jules Marey saved Christmas.


[ejm]: https://en.wikipedia.org/wiki/%C3%89tienne-Jules_Marey
[tuf]: https://books.google.co.uk/books/about/The_Visual_Display_of_Quantitative_Infor.html?id=BHazAAAAIAAJ<Paste>
[d3v]: https://bl.ocks.org/mbostock/5544008


<script src="https://d3js.org/d3.v3.min.js"></script>
<script>
var tsvLocation = "https://gist.githubusercontent.com/charlienewey/81693128c851f150de50dc4ad3b371b0/raw/ce83b6d1ffdf2d097e0e5d5d46b357a5c62b4fa2/schedule.tsv";
var stations = []; // lazily loaded
var formatTime = d3.time.format("%Y-%m-%d %H:%M");

var margin = {top: 20, right: 30, bottom: 20, left: 100},
    width = 620 - margin.left - margin.right,
    height = 450 - margin.top - margin.bottom;

var x = d3.time.scale()
  .domain([parseTime("2017-12-21 00:00"),
           parseTime("2017-12-28 00:00")])
  .range([0, width]);

var y = d3.scale.linear()
  .range([0, height]);

var xAxis = d3.svg.axis()
  .scale(x)
  .ticks(8)
  .tickFormat(d3.time.format("%b %d"));

var line = d3.svg.line()
  .x(function(d) { return x(d.time); })
  .y(function(d) { return y(d.station.distance); });

var svg = d3.select("#visualisation").append("svg")
  .attr("width", width + margin.left + margin.right)
  .attr("height", height + margin.top + margin.bottom)
  .append("g")
  .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

svg.append("defs").append("clipPath")
  .attr("id", "clip")
  .append("rect")
  .attr("y", -margin.top)
  .attr("width", width)
  .attr("height", height + margin.top + margin.bottom);

d3.tsv(tsvLocation, type, function(error, trains) {
  y.domain(d3.extent(stations, function(d) { return d.distance; }));

  var station = svg.append("g")
    .attr("class", "station")
    .selectAll("g")
    .data(stations)
    .enter().append("g")
    .attr("transform", function(d) { return "translate(0," + y(d.distance) + ")"; });

  station.append("text")
    .attr("x", -6)
    .attr("dy", ".35em")
    .text(function(d) { return d.name; });

  station.append("line")
    .attr("x2", width);

  svg.append("g")
    .attr("class", "x top axis")
    .call(xAxis.orient("top"));

  svg.append("g")
    .attr("class", "x bottom axis")
    .attr("transform", "translate(0," + height + ")")
    .call(xAxis.orient("bottom"));

  var train = svg.append("g")
    .attr("class", "train")
    .attr("clip-path", "url(#clip)")
    .selectAll("g")
    .data(trains.filter(function(d) { return /[CS]/.test(d.type); }))
    .enter().append("g")
    .attr("class", function(d) { return d.type; });

  var lastStops = {};
  train.append("path")
    .attr("d", function(d) {
      d.stops = d.stops.sort(function (a, b) {
        return a.time - b.time;
      })

      if (lastStops[d.type]) {
        d.stops.unshift(lastStops[d.type]);
        d.stops.unshift(lastStops[d.type]);
      }
      lastStops[d.type] = d.stops[d.stops.length - 1];

      return line(d.stops);
    });

  train.selectAll("circle")
    .data(function(d) { return d.stops; })
    .enter().append("circle")
    .attr("transform", function(d) { return "translate(" + x(d.time) + "," + y(d.station.distance) + ")"; })
    .attr("r", 2);
});

function type(d, i) {
  // Extract the stations from the "stop|*" columns.
  if (!i) for (var k in d) {
    if (/^stop\|/.test(k)) {
      var p = k.split("|");
      stations.push({
        key: k,
        name: p[1],
        distance: +p[2],
        zone: +p[3]
      });
    }
  }
  return {
    type: d.type,
    stops: stations
    .map(function(s) { return {station: s, time: parseTime(d[s.key])}; })
    .filter(function(s) { return s.time != null; })
  };
}

function parseTime(s) {
  var t = formatTime.parse(s);
  if (t != null && t.getHours() < 3) t.setDate(t.getDate() + 1);
  return t;
}
</script>
