# SPARQL kernel #

This module installs a Jupyter kernel for [SPARQL][]. It allows sending queries 
to an SPARQL endpoint and fetching & presenting the results in a notebook.

It is implemented as a [Jupyter wrapper kernel][], by using the Python 
[SPARQLWrapper][] & [rdflib][] packages.


## Requirements ##

The kernel has only been tried with Python 2.7 and Jupyter 4.1. It might work 
with Python 2.6 (perhaps with some tiny changes); it is unlikely to work with 
Python 3. Most of the problems will come with unicode buffers, unicode content 
(typically UTF-8) is common in SPARQL responses.

The above mentioned [SPARQLWrapper][] & [rdflib][] are dependencies (they are 
marked as such, so they will be installed with the package if needed). An 
optional dependency is [Graphviz][], needed to create diagrams for RDF result 
graphs (Graphviz's `dot` program must be available for that to work).


## Installation ##

...

## Syntax ##

The kernel implements the standard SPARQL primitives: `SELECT`, `ASK`, 
`DESCRIBE`, `CONSTRUCT`. Once the endpoint is defined (see magics below), just 
write a SPARQL valid query in a cell and execute it; the query will be sent to 
the endpoint and the results printed out.

The kernel features keyword autocompletion (TAB key), as well as contextual 
help (Shift-TAB). This is unfinished work: completion is currently done as 
isolated SPARQL keywords (no SPARQL syntax context is used) and only a few 
keywords have contextual help, as of now. 

It also installs menu entries in the HELP menu pointing to SPARQL documentation.


## Output format ##

The query results are displayed in the notebook as cell results; there are a 
number of choices for the display format, controlled via magics (see below).

Each SPARQL query is immediately launched, once the results are printed out it 
is forgotten. Cells are thus completely independent from each other (except for
magics, which are persistent).


## Magics ##

A number of line magics (lines starting with `%`) can be used to control the 
kernel behaviour. These line magics must be placed at the start of the cell, 
and there can be more than one per cell.
Valid combinations are thus:
  * a cell with only a SPARQL query,
  * a cell consisting only of magics,
  * and a cell containing both magics and then a SPARQL query (but after the 
    first SPARQL keyword the cell is assumed to be in SPARQL mode, and line 
    magics will *not* be recognized as such).

Comment lines (lines starting with `#`) can be freely interspersed between 
line magics or SPARQL queries.

Magics also feature autocompletion and contextual help. Furthermore, there is 
a spacial magic `%lsmagics`; when executed on a cell it will output the list 
of all currently available magics. The same info can be obtained by requesting
contextual help (i.e. Shift-TAB) on a line containing only a percent sign).

A few of the most relevant magics are explained in the following sections. The 
complete set is always available in the notebook, by using the help or 
autocompletion features.


### `%endpoint` ###

This magic is special in the sense that is compulsory: there needs to be an 
endpoint defined _before_ the first SPARQL query is launched, otherwise the 
query will fail.

Its syntax is:

    %endpoint <url>

and it simply defines the SPARQL endpoint for all subsequent queries. 
It remains active until superceded by another `%endpoint` magic.


### `%display` ###


Sets the output format:

    %display raw | table [withtypes] | diagram [svg|png]

There are three possible display formats:
 * `raw` outputs the literal text returned by the SPARQL endpoint, in the
   format that was requested (see `%format` magic)
 * `table` generates a table with the result. The optional `withtypes`
   modifier adds to each column an additional column that shows the data
   type for each value
 * `diagram` takes the RDF graph returned (makes sense only for N3 result
   format) and generates an image with a rendering of the graph. For it to
   work, the `dot` program from GraphViz must be available in the search path. 
   The modifier selects the image format. Default is SVG, which usually works
   much better (PNG typically generates too small images)




  [SPARQL]: https://www.w3.org/TR/sparql11-overview/
  [Jupyter wrapper Kernel]: http://jupyter-client.readthedocs.io/en/latest/wrapperkernels.html
  [SPARQLWrapper]: https://rdflib.github.io/sparqlwrapper/
  [rdflib]: https://github.com/RDFLib/rdflib
  [Graphviz]: http://www.graphviz.org/
