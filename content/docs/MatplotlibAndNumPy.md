# An introductory glossary to Matplotlib and Numpy. 

This is what I hope it becomes a personal summary for Matplotlib, Numpy, SciPy, and SciKit-Learn.

# Some reminders.

* Recall that python has the option `list_name.index(element_value)` which returns the index in this list of this element. 

## The Matplotlib's introductory glossary.

* First do the importings:
{{<highlight python>}}
    import matplotlib as mpl
    import matplotlib.pyplot as plt
{{</highlight>}}

Recall that the `plt.plot` command does not plot anything by itself. It is more an intention preparing the plot, which prepares the commands that will give to plot when one calls `plt.show()` (without arguments!).

* In order to plot a curve using some lists of _X_ and _Y_ values, use `plt.plot(X,Y)`. This automatically joins the points trying to represent a curve. In order to plot only the points, a **scatter** plot, use `plt.scatter(X,Y)`.

* In order to plot a **bar graph** use `plt.bar(X,Y)` where _X_ is where the bars begin, and the _Y_ is the heights of the bars. Analogously, one can use `plt.hbar(X,Y)` to make a horizontal bar graph. One uses the option `width` to customize the `width` of the bars, the option `color` to customize the colors, the option `bottom` to graph a raised bar (used to graph a second bar over the first,) or graph horizontal bar graphs with one set of possitive heights and one set of negative heights to compare. 
   
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
plt.hbar(X, Y_1, color = 'b')
plt.hbar(X, -Y_2, color = 'r') 
plt.show()
{{</highlight>}}

* In order to plot a **pie** use `plt.pie(Y)` 

* In order to do a **histogram** use `plt.hist(Y, bins = N)`, which splits the range into _N_ equal bins and plots how many _y_-values are in each bin. 

* In order to do a **boxplot** use `plt.boxplot(Y)`. Recall that this shows the median, a box from the extreme quartiles, and the whiskers 1.5 IQR from the quartiles. Outliers are shown with a cross marker.

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

* Both _Plots_ and _Scatter plots_ have the `marker` option and can be chosen from: 'o', '^', 'x', '.',  and 's' for square. For _Scatter plots_, the option `s` controls the size of the marker. In _plots_, one also have `markevery` to mark only certain dots.

* One can also **personalized markers** using the module `mpl.path` as `mpath`. Make a list of _X_ values, a list of _Y_ values, and add a 0 to each of them. Make a code list starting with 1 meaning **mpath.Path.MOVETO**, followed by a lot of 2's meaning **mpath.Path.LINETO**, and a final code 79 meaning **mpath.Path.CLOSEPOLY**. If they were put together, use `X,Y,codes = zip(*listName)` to split the lists, and use `my-marker = mpath.Path(np.asarray((X,Y)).T, codes)`. Now you can pass this to the the option `marker`. Other options are:
    - `markersize` for plot or `s` for scatter
    - `markeredgewidth` for plot only. 
    - `color` for the border and `markerfacecolor` for the face in _plot_, and `markeredgecolor` or `edgecolor` for the border and  `color` for the face in _scatter_
 
* In order to change **colors** one has the option of using _triplets_ of reals in [0,1] representing the red, blue, and green; or to use _quadruplets_ where the last entry is the transparency; or to use a single real in [0,1], which is a grey scale where _0_ is _black_ and _1_ is _white_. 

* One can also use **predetermined colors**: 'b' for blue, 'g' for green, 'r' for red, 'c' for cyan, 'm' for magenta, 'y' for yellow, 'k' for black, and 'w' for white.

* One can also use a list of colors `colors = my_color_list` and the option will cycle through this list. 

* The option `edgecolor` is available on bars and histograms, but not on pies.

* The module `matplotlib.cm` is a module of predetermined maps from **numbers to colors**: for example `cm.hsv` makes it look like a rainbow. 

* For example, on a _scatter plot_, the option `c` is _color_, a _color sequence_, or a sequence of numbers that requires a map to colors, so one needs to add an option like `cmap = cm.hsv`.

* A more refined example is to customize `cmap = cm.ScalarMappable(col.Normalize(0, 99), cm.binary)`. The first argument normalizes our values to the interval (0,99), and the second tells it to map lower values to greys close to white, and upper values close to black. 

* The module `mpl.rc` sets up global variables to use in the plots. The general notation is `mpl.rc('figure_name', feature_name = value)`. For example, in order to set the `lines.linewidth`, one uses:

{{<highlight python>}}
    mpl.rc('lines', linewidth = 1.5)
{{</highlight>}}

* Other properties are `figure.facecolor`, `axes.facecolor` for the background; `axes.edgecolor`, `xtick.color`, `ytick.color`, `text.color`, `figure.edgecolor` for the annotations color; `axes.color-cycle`.

* Alternatively, one can define this in a plain text file **matplotlibrc** in the same folder as the python-script or in the folder returned by `mpl.get\_configdir()` with one line per property of the form **property : value**  and thus it can be used in all the subsequent plots.

* In order to **draw a picture** from a file containing the colors of each pixel, one can use for example:
{{<highlight python>}}
    plt.imshow(file, cmap = mpl.cm.binary, interpolation = "nearest")
    plt.axis("off")
    plt.show()
{{</highlight>}}



## The NumPy library's basic glossary.
* First import it.
{{<highlight python>}}
    import numpy as np
{{</highlight>}}

* The major use of _NumPy_ is for their arrays, which contain the data (values) and metadata (internal description.) The major distinction with a Python list is that each array has only one numerical type of entries. One can pass the option `dtype` in the construction of arrays to specify it, or call the routine `.dtype` to find it. The most common types are: bool; inti; int8/uint8 (8,16,32,64) where **u** means unsigned; float/16 (16,32,64 or just float); complex64 (64,128 or just complex). Also it has compatibility with the _Numeric_ library, so it allows to use _Numeric_ types: i integer, u unsigned integer, f single prec float, d double precision float, b bool, D complex, S string, U unicode, V void.

    The command `stypeDict.keys()` return a list of all types. The command `dtype(type_character).type` returns the full-word of type. The command `dtype(type_name).char` return the single character type. Moreover, one can use **records** or personalized types, e.g. `my_type = dtype[('name', dataType)]; myitem = array([(tuple1)], dtype=t)`.

* The most common way to create arrays is through the use of `np.arange`. For example `np.arange(n)` has the same data as `range(n)` and `np.arange(m,n)` has the same data as `range(m,n)`. 

* The most common ways to append arrays are through the use of:
    - The routine `append(array, values, axis=None)`, which requires the values to have the same dimension. _Warning_ contrary to the `append` in Python, this is not in place.
    - The routine `block(list_of_arrays)`, which joins them.

* Among the most common ways to split arrays is:
    - The routine `array_split(array, indices, axis=0)`, which allows non-equal subdivision of the array and makes the latest ones shorter.

* The **dimension** of an array can be found by the method `shape` (without parenthesis). It can be changed with the command `reshape((n1,n2,...,nk))` (not in place) or `resize(tuple)` (in place) as long as the number of entries matches the product of the tuple entries. In particular `ravel()` flattens the array (not in place,) and `flatten()` also allocates memory (not in place.) Using _Linear Algebra_ notion, one has the methods `transpose()` or just `T` which reverse the individual dimensions.

* Contrary to _Python_, one can operate (addition, subtraction, multiplication, division, checking equality) a number to an array, e.g. `my_array + number`, and it does perform an entrywise operation, i.e. it operates every entry of the array. Similarly, checking for equalities with `my_array == my_array_2` returns a boolean array comparing corresponding entries. 

*  The **indexing** in Numpy is also slightly different `[i_1,i_2]` means what Python has for `[i_1][i_2]`. In the same way, **slicing** is done through square brackets and a comma separated list of slices. For example `[:,[2:4],[::-1]]` means elements whose 1st dimension is unrestricted, 2nd dimension is either 2 or 3, and the last dimension is reversed. 
    - The routine `hsplit(data,n)` or `split(data,n,axis=1)` homogeneously splits the columns into n subarrays.
    - The routine `split(data,n)` or `split(data,n,axis=0)` homogeneously splits the rows into n subarrays.
    - The `axis` option indicates on which dimension we are performing the splitting. 
    \end{itemize}


* The **Stacking** options are: 
    -The routine `np.hstack((a,b))` or `np.concatenate((a,b), axis=1)` appends two arrays horizontally. `column\_stack((a,b))` changes list to columns and appends them horizontally, but on multidimensional arrays, it is the same as `hstack`.
    -The routine `np.vstack((a,b))` or `np.concatenate((a,b), axis=0)` appends two arrays vertically. `row\_stack((a,b))` works as well as `vstack`.
     - `r_` allows faster stacking by listing entries by themselves, arrays described as `begin-inclusive:end-inclusive:number-of-entries+j`. Similarly `c_`. The difference is that for 2-dimensional arrays `r_` stacks them by rows and `c_` stacks them by columns. 


* In order to create **random arrays** where the entries have been created with a specific distribution use: `np.random.command(length)`. 
    
    - `random.rand`, `random_sample`, `random`, `randf`, `sample` choose from the uniform distribution in some version of the interval [0,1].
    - `randn,standard\_normal` chooses from the Gaussian normal distribution, i.e. median 0 and standard deviation 1.
    - `randint` (half-open), `random\_integers` chooses from the uniform distribution on the integers using the same ranges as `arange`.
    \end{itemize}
    - For other distributions listed see: `uniform`, `choice`, `bytes`, `shuffle`, `permutation`, `(multivariate_)normal` with symmetric positive-definite covariance matrix, `lognormal`, `logseries`, `dirichlet`, `beta`, `(standard_)gamma`, `laplace`, `standard_cauchy`, `(standard_)exponential`, `geometric`, `hypergeometric`, `logistic`, `(negative_)binomial`, `multinomial`, `(noncentric_)chisquare`, `(noncentric_)f`, `pareto`, `poisson`, `power`, `rayleigh`, `gumbel`, `standard_t`, `triangular`, `vonmises`, `wald`, `wibul`, `zipf`.
    - For a more general use see the documentation of `RandomState`.



* Some basic statistics of an array can be read with `np.mean(data_array)` and `np.std(data_array)` for the **mean** and **standard deviation**. 

* In order to make arrays splitting an interval into equal sub-intervals use
`np.linspace(a,b,n)` to return an array with _n_ points arithmetical-equally spaced, e.g. `a + (b-a)/(n-1) * i`. Similarly the routine `np.logspace(a,b,n)` returns _n_ points geometrical-equally spaced, e.g. `10**(a + (b-a)/(n-1) * i)`

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
  

## Additional NumPy routines and commands

* The **Polynomials** are described by their descending coefficients, e.g. `np.poly1d([3,2,1])` means the quadratic polynomial `3*x**2 + 2*x + 1`. One can derived them with `.deriv()`, indefinite-integrate them with `.integ(n)`, which also sets the constant to _n_, evaluate them in numbers or in intervals `my_poly([a,b])`

* One can **Vectorize** a function, which means given a function `f(arg1,arg2)`, use `vf = np.vectorize(f)`, and now one can pass two arrays of the same length and `vf` will compute `f` one each par of corresponding entries. _WARNING_: Sometimes running a vectorized function using NumPy in Python can run as fast as writing a loop in Cython, and it is much simpler.

* One can test the structure of an array with `np.iscomplex` or `np.isreal`, which returns an array. Or test the structure of all its elements combined with `np.iscomplexob`, `np.isrealobj`. Alternatively, one can split into the real and imaginary parts using `np.real` and `np.imag`.

* In order to define **piecewise-functions** one uses `np.select(conditions\_array, functions\_array)`, e.g. `np.select([x<1,x>2], [x**2, x**3])`.

* While for Python 3.0, one enjoys the `help(command)`, which provides the documentation for command, NumPy has the `np.info(command)`.


# The begining of SciPy

* See the library **scipy.special** for many mathematical physics functions: airy, elliptic, bessel, gamma, beta, hypergeometric, parabolic cylinder, mathieu, spheroidal wave, struve and kelvin. It include some stats functions which are used more commonly through the **scipy.stats** module. Additionally, the module **scipy.special.cython_special** offers cython versions of many of this special functions.

* SciPy requires to import independently each of the following packages: `cluster`, `constants`, `fftpack`, `integrate`, `interpolate`, `io`, `linalg`, `ndimage`, `odr`, `optimize`, `signal`, `sparse`, `spatial`, `special`, `stats`. 
 
* For example `scipy.integrate.quad(lambda x: f(x), a, b)` returns the definite integral of _f_ from _a_ to _b_. Similarly, the commands `dblquad`, `tplquad`, `nquad` return the _doble_, _triple_ and _n-fold_ integrals. The commands `trapz`, `cumtrapz`, `simps`, `romb` use _trapezoidal_, _trapezoidal cumulative_, _simpson_, and _romberg_ approximations for functions given some fixed samples. The commands `odein`, `ode` are numerical integrators of ODE systems (see VODE and ZVODE routines for more general commands).
