# `physrev_mplstyle`

## Introduction
This is a [Matplotlib](matplotlib.org) style sheet designed to help one produce 
publication-quality figures for the [APS](www.aps.org) [Physical Review](journals.aps.org) journals easily.
In particular:

* It uses `Computer Modern` as font.
* It sets axes and tick labels font sizes to match that of `revtex4-2`'s default 10pt. Labels have some "breathing" space from the figure's frame.
* Figure's width is set as to use of most of the [column-width space](https://tex.stackexchange.com/questions/184045/how-many-pixels-inches-or-centi-meters-is-a-linewidth-in-a-two-columned-article) available when using the double-column format.
* Figure's heigth is such that `width / height ~ golden ratio`.
* Uses [Mathematica](www.wolfram.com/mathematica)'s default color scheme for up to nine colours.
* Has additional tweaks to the default `legend`, `grid` and line styles.

## Usage
See the example notebook `plt.ipynb`. First, save `physrev.mplstyle` somewhere in your computer and use as preamble:

```python
import matplotlib.pyplot as plt

%matplotlib inline
%config InlineBackend.figure_format='retina' # Optional

plt.style.use('physrev.mplstyle') # Set full path to if physrev.mplstyle is not in the same in directory as the notebook
plt.rcParams['figure.dpi'] = "300"
```

The line `plt.rcParams['figure.dpi'] = "300"` is for convenience; it makes the otherwise small-sized figure appear larger 
when printed out in the notebook.

A basic example is the following:

```python
f, ax = plt.subplots(1, 1)

x = np.linspace(0, 1)
s = np.flip(np.arange(1, 1 + 9/10., 1/10.))

for slope in s:
    ax.plot(x, slope * x, label="{:0.1f}".format(slope))

ax.set_xlim(0, 1);
ax.set_ylim(0, 2);

ax.set_xlabel(r'$x$')
ax.set_ylabel(r'$y = s \, x$')

ax.legend(ncol=3)
ax.get_legend().set_title(r"Values of $s$")  
ax.grid()

plt.savefig('tmp.pdf')
```

This produces the following figure:

<p align="center">
<img src="/figs/example.png" width=75%>
</p>

We can now use this image in a `tex` document. Usually, my `documentclass` has:

```tex
\documentclass[aps, 10pt, prd,
               notitlepage, twocolumn, superscriptaddress,
               longbibliography,
               nofootinbib, floatfix]{revtex4-2}
```

and we can insert our figure as

```tex
\begin{figure}[htb]
  \includegraphics[width=\columnwidth]{figs/tmp.pdf}
  \caption{Example of figure in a document using \texttt{revtex4-2}.}
  \label{fig:tmp}
\end{figure}
```

<p align="center">
<img src="/figs/in_tex.png" width=90%>
</p>

And that's it.

## Resources for the plotting aficionado

* Matplotlib documentation on [customization](https://matplotlib.org/stable/users/explain/customizing.html).
* See also my previous style sheets at the `mplstyle` [repo](https://github.com/hosilva/mplstyle/tree/master).
* You may be interested in checking also `SciencePlots` [package](https://github.com/garrettj403/SciencePlots).
