# ode-plotter

Simple plotting utility for nonlinear dynamics attractors and other ODEs using the Plotly graphing library.

Try it out [here](https://blackguard.github.io/ode-plotter/ode-plotter.html)

# Usage
Load the file ode-plotter.html in a modern Web browser such as Firefox or Chromium.

Define and return a "step" function in the input and click then "Go".

You can rotate the graph while running by dragging it, but for full control you must Stop the process.

# Optimize startup
If you experience startup delays, you can download the Plotly code instead of loading it from the CDN each time.  Download the code from https://cdn.plot.ly/plotly-2.8.3.min.js and then change the <script> element src attribute to point to your donwloaded copy.
