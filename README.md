# ode-plotter

Simple plotting utility for nonlinear dynamics attractors and other ODEs using the Plotly graphing library.

Try it out [here](https://blackguard.github.io/ode-plotter/ode-plotter.html)

[Repository on github](https://github.com/blackguard/ode-plotter)

# Usage
Load the file ode-plotter.html in a modern Web browser such as Firefox or Chromium.

Define and return an "equation definition" in the input area at the top of the page, and then click "Go".

You can rotate the graph while running by dragging it, but for full control you must Stop the process.

# Optimizing startup
If you experience startup delays, you can download the Plotly code instead of loading it from the CDN each time.  Download the code from https://cdn.plot.ly/plotly-2.8.3.min.js and then change the <script> element src attribute to point to your donwloaded copy.

The next section describes how to do this more easily.

# Saving your work
If you save the page on your computer as "Web page, complete", then the changes you make in the input area will be saved to that local copy.  Additionally, the Plotly library will be cached locally, and that permits offline use and makes startup faster.

Note: this has been tested on Chromium and Firefox on Ubuntu 20.04.  I hope that it will work equally well on other platforms and/or other "modern" browsers.

# Defining Equations

Equations are defined in the input area, by returning an equation definition object.

## Equation Definition Properties

| Name          | Required? | Type               | Description |
| :------------ | :-------: | :----------------- | :---------- |
| title         |           | String             | title for the plot |
| f             | x         | Function           | (x: number[], params?: any) => number[] |
| iter          |           | Iterable&#124;Generator | iterable, array or generator function returning Config objects for each iteration |
| iter_delay    |           | number[]           | delay between iterations (milliseconds) |

... or any field from a Config object

## Config Object Properties

| Name          | Required? | Type                 | Description |
| :------------ | :-------: | :------------------- | :---------- |
| extrapolator  |           | Function             | (dt: number, f: Function, x: number[], params: any) => number[] |
| dt            |           | number[]             | time step |
| x0            |           | number[]             | initial value |
| params        |           | any                  | parameters that will be passed to f |
| skip          |           | non-negative integer | number of time steps to skip before beginning plot (default: 0) |
| steps         |           | non-negative integer | number of time steps to plot after skipped time steps (default: Infinity) |
| point_cloud   |           | PointCloud           | definition of point cloud to be plotted after main equation is plotted |

## PointCloud Properties

| Name          | Required? | Type                 | Description |
| :------------ | :-------: | :------------------- | :---------- |
| n             | x         | positive integer     | number of point cloud points |
| c             | x         | number[]             | center of point cloud |
| r             | x         | number               | maximum radius around center for points |
| dt            |           | number               | time step (default: time step from current config) |
| steps         |           | non-negative integer | number of time steps to run point cloud (default: Infinity) |

## Examples

The following examples reference an extrapolator.  An example is:

```javascript
function runge_kutta_extrapolator(dt, f, x, params) {
    // This is an implementation of the fourth-order Runge-Kutta method
    // as presented in Nonlinear Dynamics and Chaos, Second Edition, by
    // Steven H. Strogatz, page 34.
    // This implementation works for x with arbitrary dimension.
    const k1 = f( x,                                params ).map( xidot => xidot*dt );
    const k2 = f( x.map( (xi, i) => xi + k1[i]/2 ), params ).map( xidot => xidot*dt );
    const k3 = f( x.map( (xi, i) => xi + k2[i]/2 ), params ).map( xidot => xidot*dt );
    const k4 = f( x.map( (xi, i) => xi + k3[i]),    params ).map( xidot => xidot*dt );
    return x.map( (xi, i) => xi + (k1[i] + 2*k2[i] + 2*k3[i] + k4[i])/6 );
}
const extrapolator = runge_kutta_extrapolator;
```

### Lorenz attractor

```javascript
return {
    title: 'Lorenz Attractor',
    extrapolator,
    dt: 0.005,
    x0: [3, 3, 1],
    params: { sigma: 10, r: 28, b: 8/3 },
    f:  ([x, y, z], {sigma, r, b}) => [
            sigma*(y - x),
            r*x - y - x*z,
            x*y - b*z,
        ]
};
```

### Rössler attractor iterating its _a_ parameter over several plots

```javascript
return {
    extrapolator,
    dt: 0.02,
    x0: [10, 10, 1],
    f:  ([x, y, z], {a, b, c}) => [
            -y - z,
            x + a*y,
            b + z*(x - c),
        ],
    params: { a: 0.2, b: 0.2, c: 5.7 },
    skip:  1000,
    steps: 5000,
    iter: function* (def) {
        for (let a = 0.1; a < 0.2; a += 0.001) {
            yield { params: { ...def.params, a } }
        }
    },
    iter_delay: 1000,
};
```

### Lorenz attractor with point cloud

```javascript
return {
    title: 'Lorenz Attractor With Point Cloud',
    extrapolator,
    dt: 0.005,
    x0: [3, 3, 1],
    params: { sigma: 10, r: 28, b: 8/3 },
    f:  ([x, y, z], {sigma, r, b}) => [
             sigma*(y - x),
             r*x - y - x*z,
             x*y - b*z,
        ],
    skip:  1000,
    steps: 10000,
    point_cloud: {
        n: 6000,
        c: [1, 1, 1],
        r: 0.05,
        dt: 0.05,
    }
};
```
