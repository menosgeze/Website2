# A descent summary introducing to some Python libraries.

This is a personal summary for Matplotlib, Numpy, SciPy, and SciKit-Learn.

## The Matplotlib library's basic sintax.

* First 
`import matplotlib as mpl`,
`import matplotlib.pyplot as plt`. 

Recall that the `plt.plot` command does not plot anything by itself. It is more an intention preparing the plot, which will be presented by calling `plt.show()` without arguments.

* In order to plot a curve using some lists of $X$ and $Y$ values, use $`plt.plot(X,Y)`. This automatically joins the points trying to represent a curve.

* In order to plot only the points, a **scatter** plot,  using the list of $X$ and $Y$ values, use $plt.scatter(X,Y)$.

* In order to plot a **bar graph** use `plt.bar(X,Y)` where $X$ is where the bars begin, and the $Y$ is the heights of the bars. Alternatively, one can use `plt.hbar(X,Y)` to make a horizontal bar graph. One uses the option `width` to customize the `width` of the bars, the option `color` to customize the colors, the option `bottom` to graph a raised bar (used to graph a second bar over the first,) or graph horizontal bar graphs with one set of possitive heights and one set of negative heights to comapre. 
   
*An example* to graph several bar graphs in a single set of axis could be done by:
{{<highlight python>}}
n = 2
i = 0; plt.bar(X + i/(n+1), Y_1, width = 1/(n+1), color = 'b')
i = 1; plt.bar(X + i/(n+1), Y_2, width = 1/(n+1), color = 'r') 
plt.show()
{{</highlight>}}

*An example* to graph the second bars on top of the first ones:
{{<highlight python>}}
plt.bar(X, Y_1, color = 'b')
plt.bar(X, Y_1 + Y_2, bottom = Y_1, color = 'r') 
plt.show()
{{</highlight>}}

*An example* to graph the bars opposite to each other:
{{<highlight python>}}
plt.bar(X, Y_1, color = 'b')
plt.bar(X, -Y_2, color = 'r') 
plt.show()
{{</highlight>}}

* In order to plot a **pie** use `plt.pie(Y)` 

* In order to graph a **histogram** use `plt.hist(Y, bins = N)`, which splits the range into _N_ equal bins and plots how many _y_-values are in each bin. 

* In order to graph a **boxplot** use `plt.boxplot(Y)`. Recall that this shows the median, a box from the extreme quartiles, and the whiskers 1.5 IQR from the quartiles. Outliers are shown with a cross marker.

In particular, saving the result into a variable, makes the variable a dictionary containing keys: whiskers, caps, boxes, medians, fliers, mean. The values are lines. Use `.set-color()` to customize the colors.

* Another graph is the **Triangulations** which require slightly more than a single command:

{{<highlight python>}}
    # Creating the triangles 
    triangles = mpl.tri.Triangulation(X,Y)
    # Intention to plot them 
    plt.triplot(triangles)
{{</highlight>}}

## Some Customizables in Matplotlib
* _Plots_ have an option `linestyle` taken from the list: 'solid', 'dashed', 'dashdot', 'dotted'. In _bars_, this same option means _borderlines_, where the filling patters are chosen by `hatch` from 'X', '+', '-', '/', '\', '|', 'o', 'O', '.', '*'.

* _Plots_ also have `linewidth` to determine the thickness set.

* _Scatter plots_ have the `marker` option and can be chosen from: 'o', '^', 'x', '.'$,  and $'s'$ for square. The option `s` controls the size of the marker. In _plots_, one also have `markevery` to mark only certain dots.

* One can also **personalized markers* using the module `mpl.path` as `mpath`. 
Make a list of vertices of the form `(x,y,2)` where $2$ is a code for 
**mpath.Path.LINETO**. Change the first one to `(x,y,1)` for
**mpath.Path.MOVETO**. And add the last point `(0,0,79)` for **mpath.Path.CLOSEPOLY**. Then use `u,v,codes = zip(*listName)` to write the X,Y, codes lists, and use `my-marker = mpath.Path(np.asarray((u,v)).T, codes)`. Now you can pass this to the
the option `marker` option. Other options are:
\begin{itemize}
    \item `markersize` for plot or `s` for scatter
    \item `markeredgewidth` for plot only. 
    \item `color` for the border and `markerfacecolor` for the face in _plot_, and `markeredgecolor` or `edgecolor` for the border and  `color` for the face in _scatter_
\end{itemize}

 
* In order to change **colors** one has the option of using _triplets_ of reals in [0,1] representing the red, blue, and green; or to use _quadruplets_ where the last entry is the transparency; or to use a single real in [0,1], which is a grey scale where _0_ is _black_ and _1_ is _white_. 

Alternative, one can use **predetermined colors**: 'b' for blue, 'g' for green, 'r' for red, 'c' for cyan, 'm' for magenta, 'y' for yellow, 'k' for black, and 'w' for white.

Moreover, we use the option **colors** with a previously created list `colors = my_color_list`, and if there is not enough colors in our list, then the option cycles through the list. 

* The option **edgecolor** is available on bars and histograms, but not on pies.

* The module `matplotlib.cm` is a module of predetermined maps from **numbers to colors**: for example `cm.hsv` makes it look like a rainbow. 

For example, on a _scatter plot_, the option **c** is _color_, a _color sequence_, or a sequence of numbers that requires a map to colors, so one needs to add an option like `cmap = cm.hsv`.

A more refined example is to customize `cmap = cm.ScalarMappable(col.Normalize(0, 99), cm.binary)`. The first argument normalizes our values to the interval (0,99), and the second tells it to map lower values to greys close to white, and upper values close to black. 

* The module `mpl.rc` sets up global variables to use in the plots. The general notation is `mpl.rc('figure_name', feature_name = value)`. For example, in order to set the `lines.linewidth`, one uses:

{{<highlight python>}}
    mpl.rc('lines', linewidth = 2)
{{</highlight>}}

Other properties are `figure.facecolor`, `axes.facecolor` for the background; `axes.edgecolor`, `xtick.color`, `ytick.color`, `text.color`, `figure.edgecolor` for the annotations color; `axes.color-cycle`.

Alternatively, one can define this in a plain text file **matplotlibrc** in the same folder as the python-script or in the folder returned by `mpl.get\_configdir()` with one line per property of the form **property : value**  and thus it can be used in all the subsequent plots.

## The NumPy library's basic sintax.
* First `import numpy as np`

* Recall that python has the option `list_name.index(element_value)` which returns the index in this list of this element. 


* In order to create **random arrays** where the entries have been created with a specific distribution use: `numpy.random.command(length)`. For example: 
`randn` or `standard_normal` takes the entries from the normal distribution, `randint(M, length)` take integers between _0_ and _M-1_ chosen from the uniform distribution..

* Some basic statistics of an array can be read with `np.mean(data_array)` and `np.std(data_array)` for the **mean** and **standard deviation**. 

* In order to make arrays splitting an interval into equal sub-intervals use
`np.linspace(a,b,n)` to return an array with _n_ points equally arithmetically spaced where the first is _a_ and the last is _b_.

* In order to **load text** containing 2 numbers per line, there is various alternatives, I here choose the 2 most appealing to me:

    - The first alternative is pure python:
    {{<highlight python>}}
        f = open('filename.ext', 'r')
        X, Y = zip(*[[float(s) for s in line.split()] for line in f.readlines()])
    {{</highlight>}}

    - The second one is using numpy:
    {{<highlight python>}}
        data = np.loadtxt('filename.ext')
        X, Y = data[:,0], [:,1]
    {{</highlight>}}
    _Warning_: `loadtext` read strings as b-strings, so if the file has the headers, one may take care of them using `converter = {colNumber, funct}` where the function should take b-string and maps them say to their index in a label list, i.e. `label_list.index(some_b_string)`
    _Note_: Another important option is `usecols` which take a list of the columns that one wants to analyze.
  







