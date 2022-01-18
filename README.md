# ode-plotter

Simple plotting utility for nonlinear dynamics attractors and other ODEs using the Plotly graphing library.

Try it out [here](https://blackguard.github.io/ode-plotter/ode-plotter.html)

[Repository on github](https://github.com/blackguard/ode-plotter)

# Usage
Load the file ode-plotter.html in a modern Web browser such as Firefox or Chromium.

Define and return an "equation definition" in the input area at the top of the page, and then click "Go".

You can rotate the graph while running by dragging it, but for full control you must Stop the process.

# Optimize startup
If you experience startup delays, you can download the Plotly code instead of loading it from the CDN each time.  Download the code from https://cdn.plot.ly/plotly-2.8.3.min.js and then change the <script> element src attribute to point to your donwloaded copy.

The next section describes how to do this more easily.

# Saving your work
If you save the page on your computer as "Web page, complete", then the changes you make in the input area will be saved to that local copy.  Additionally, the Plotly library will be cached locally, and that permits offline use and makes startup faster.

Note: this has been tested on Chromium and Firefox on Ubuntu 20.04.  I hope that it will work equally well on other platforms and/or other "modern" browsers.
