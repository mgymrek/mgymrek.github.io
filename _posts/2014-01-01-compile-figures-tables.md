---
layout: post
title: Automatically compiling figures, tables, and legends for your next manuscript
categories: python
tags: python utils
date: 2014-01-01 21:35:00
---
While I was rearranging the figures for a manuscript for the millionth time, this [graph](http://www.howtogeek.com/102420/geeks-versus-non-geeks-when-doing-repetitive-tasks-funny-chart/) came to mind (replace "task size" with "# times to do the task", [This XKCD comic](http://xkcd.com/1205/) is also relevant):

![geeks-repetitive-tasks](http://www.howtogeek.com/geekers/up/sshot4f07447e46648.jpg)

The usual workflow that ends up happening for all the mansucripts I've been involved in goes something like this:
1. Spend several months (years?) on a project, generating figures and tables of data along the way. Often these will be in a variety of locations/formats.
2. When it comes time to write up the paper, figure out which figures and tables you want to go in the main text and the supplementary material. Spend hours manually formatting in Illustrator/Word. You're sure this is going to be the final version, so it doesn't matter that you're spending hours to manually format everything.
3. Decide that you want to move the figures around. Completely redo half of the manual formatting work you did in the previous step. It's ok because this is *really* the final version.
4. Repeat step 3 approximately 10 or more times.
5. Eventually: submit the paper. Hours (days?) of your life wasted clicking and dragging around Illustrator and word files.

Sound familiar?

So this time around I decided to take all the hours I would usually spend redoing all of this and replace it by a day of coding to automate the whole process. Since I recently converted all my figures to be made by [IPython notebooks](http://www.ipython.org/notebook.html), this was easier than it would have been otherwise. The task at hand is to:
1. Specify which figures go where. (e.g. this plot is for Figure 1A, that plot is for Supplemental Figure 2, this data frame has the data for Table 1, etc.).
2. Parse the IPython notebook to regenerate the main figures as PDFs and arrange them in the desired configuration (e.g. 2x2 grid, 3x1 grid, etc.), with appropriate "A",'B","C","D", etc. letterings.
3. Spit out the tables and formatted legends to a docx file.
3. Parse the IPython notebook to generate the supplemental figures as PNGs in the desired format. Spit out supplemental figures, tables, and formatted legends to a docx file.

After learning about some new python libraries for manipulating PDF and docx files, this turned out to be successful. The results of this endeavor are described below. 

# Overview of CompileFiguresTables.py #
To compile figures and tables from a list of IPython notebooks is as simple as:
```
python CompileFiguresTables.py \
  --nb my_ipython_notebook1.ipynb,my_ipython_notebook2.ipynb \
  --figlist fig_table_list.json \
  --out smith_etal
```

where:
* ```--nb``` is a list of IPython notebook files containing the code to generate figures and tables. Also contains legends for each plot/table. Requires a couple special lines for formatting, described below.
* ```--figlist``` is a JSON file specifying which figures and tables go where (more below).
* ```--out``` is a prefix to name output files. This will create ```smith_etal.<FigureName>.pdf``` for each main figure, ```smith_etal.maintext_legends_and_tables.docx```, and ```smith_etal.supplemental_figures_and_tables.docx```.

This code is available as a [github gist](https://gist.github.com/mgymrek/8212635). If it proves useful enough or anyone requests it I'll make a full repository out of it and add some more features I'd like to get to. If you download the gist you'll get:
* ```CompileFiguresTables.py```: the script that does all the work
* ```small-test.ipynb```: example IPython notebook to generate plots/tables
* ```example_fig_list.json```: example argument to ```--figlist``` to specify how figures are laid out.

Note this requires the Python libraries: [docx](https://github.com/mikemaccana/python-docx), [PyPDF2](https://github.com/mstamy2/PyPDF2), [matplotlib](http://matplotlib.org/api/), [pandas](http://pandas.pydata.org/)

To run the example, simply do:
```
python CompileFiguresTables.py \
  --nb small-test.ipynb \
  --figlist example_fig_list.json \
  --out test
```

And you'll get the outputs:

* [test.Figure1.pdf]({{ SITE.URL }}/test.Figure1.pdf)
* [test.maintext_legends_and_tables.docx]({{ SITE.URL }}/test.maintext_legends_and_tables.docx)
* [test.supplemental_figures_and_tables.docx]({{ SITE.URL }}/test.supplemental_figures_and_tables.docx)

Pretty fun! Hour saved right there. The example (and more) are described in detail below. The example is super simple, but you can make this as elaborate as you want.

# The example #

Let's take a look at the input files we used for the example. 

### small-test.ipynb ###
First, the IPython notebook. Open it up in your browser (recommended. For more info on using Ipython notebooks see [here](http://ipython.org/ipython-doc/stable/interactive/notebook.html)), or you can view it [here](http://nbviewer.ipython.org/urls/gist.github.com/mgymrek/8212635/raw/7b4b11e2dc4d253c7ebab0e08db1bbcc03eb03bd/small-test.ipynb?create=1)

This notebook consists of three types of cells:
* Regular IPython code cells without anything special
* Code cells with a comment in the format: ```# FIGURE: <name>``` followed by code to create either a figure or a table. (use FIGURE for both, since I added tables to this after that was already hardcoded)
* Markdown cells with: ```### LEGEND: <name> ###```

So to create a figure named "fig1", we create a code cell with the comment ```# FIGURE: fig1``` at the top. Then we create the figure (importantly, use a ```matplotlib.Axes``` object named ```ax``` to do this). Finally, we create a markdown cell to contain the legend, with heading ```### LEGEND: fig1``` ###.

Similarly, to create a table "test-table", create a code cell with the comment ```# FIGURE: test-table``` at the top. That code should output a ```pandas.DataFrame``` object. Finally, if you want you can create a legend for the table in the same way you did for the figure. If you don't make a legend, it will just be an empty string.

### example_fig_list.json ###

The second component is configuring where you want each figure and table to go. This is done using a pretty straightforward file in JSON format. The example in ```example_fig_list.json``` is hopefully self-explanatory, shown below.

```
{
    "MainText": {
        "Figures": [
            {
                "FigureName": "Figure1",
		"FigureTitle": "TestFigure1",
                "SubFigures": [
                    "fig1",
		    "fig2",
		    "fig1",
		    "fig2"
                ],
                "Layout": "(1,2),(3,4)"
            }
        ],
	"Tables": []
    },
    "Supplemental": {
         "Figures": [
            {
                "FigureName": "SuppFig1",
		"FigureTitle": "SuppFig1Test",
		"SubFigures": [
	            "fig2",
		    "fig1"
		],
		"Layout": "(1,2)"
            },
	    {
		"FigureName": "SuppFig2",
		"FigureTitle": "SuppFig2Test",
		"SubFigures": [
		    "fig1"
		],
		"Layout": "(1)"
	    }
        ],
	"Tables": [
	    {
	        "TableName": "SuppTable1",
		"TableTitle": "Testing tables",
		"Table": "test-table"
	    }
	]
    }
}
```

The names of "SubFigures" and "Table" should correspond to the figure names in the IPython notebook. 

Layout is a format string giving grid: Examples:
* A single figure: (1)
* 2x2 grid: (1,2),(3,4)
* 2x2 grid, first figure takes up whole top row: (1,1),(2,3)

Looking at the JSON above, we tell the script to make one main figure, with the name "Figure1" and title "TestFigure1", with four subplots arranged in a 2x2 grid; no main tables; two supplementary figures, and one supplementary table. Take a look at the example outputs to see how these are formatted.

And that's all there is to it! Want to change which figure goes where? No problem, make a few edits to the JSON file and rerun! The numbers in your long table have changed? No need to manually edit them in Word, just rerun the python script! Seconds vs. minutes or hours to rearrange.

# How it works #

Here is how this all works in a nutshell:

Parse the IPython notebook file(s). Break it up into:
* Code cells containing a ```# FIGURE: <name>``` comment
* Markdown cells containing ```### LEGEND: <name> ###``` headings
* All other "supporting" code cells.

Make dictionaries of name:code and name:legend text for each figure or table.

Execute all the lines of supporting code (ignore any lines with IPython magics in them, since I don't know how to execute these from a regular python script)

Parse the JSON file giving the list of figures and tables.

For each figure:

* Get the code/legend for all the subfigures.
* Create a new matplotlib figure, and add an Axes for each subfigure in the specified layout. Add "A","B","C",etc. if there is more than one subfigure.
* For each subfigure, execute the code for that subfigure, being sure to use the Axes object for that subfigure.
* Get the figure title and legend text for each subfigure. Write the legend to a word file.

For each table:

* Parse the pandas data frame into a list of rows, where each row is a list of elements. Use the docx library to write this as a table to a word file.

There were quite a few minor details to work out, but basically that's how it works! Happy to hear about potential improvements. On the wishlist for now is the ability to customize the Word document format better (font face, size, table formats, etc.).

# Appendix: Pro Tips #

1. **Add empty grid spaces to make the layout nicer**. You can add "null" plots to the list of subfigures by listing the empty string, instead of the name of a plot. I found when making a 3x1 grid it looked better to specify the Layout as (1,4),(2,4),(3,4) and specify the list of figures as "fig1","fig2","fig3","" for example.

2. **Make data dense figures as pngs instead of pdfs**. When making my figures, I realized that one of the subplots has hundreds of thousands of data points, and generates a PDF that is about 3MB. This basically crashed Adobe whenever I tried to view the pdf. I would like to still generate the rest of the subplots as pdfs, but make the data for that plot as a PNG to make the file a manageable size. You can do this by specifying "figurename:png" in the list of SubFigures. Note this only makes the *data* into a PNG. The axis and other subfigures for that figure are still generated as PDFs. This gook quite a bit of painful engineering, but it works very nicely and has saved me a lot of time.

3. **Make your matplotlib pdfs into letter size without stretching them**. This is implemented in the script, but I thought I'd point it out here because it took me a while to figure out and could be useful to anyone else trying to do the same. The problem is that the matplotlib figures I generate are not in letter size pdfs. If I specify the figure size, using ```fig.set_size_inches((8.27,11.69))``` for instance, this will stretch the figure. Instead, I just want to extend the media box to be letter size (A4) without actually modifying the figure contents. To do this, I first save my figure in matplotlib with the correct width:

{% highlight python %}
LETTERSIZE = (8.27, 11.69) # final paper size in inches
pad = 0.42 # since the bbox isn't really "tight", need this to make the size correct.
           # Probably there's a better way to do this...
fig.set_size_inches((LETTERSIZE[0]-pad, (LETTERSIZE[0]-pad)*numcols*1.0/numrows))
plt.savefig(p, bbox_inches="tight", pad_inches=0)
{% endhighlight %}

Then using the PyPDF2 library we can resize the media box to be letter size:

{% highlight python %}
pr = PyPDF2.PdfFileReader(open("my_matplotlib_pdf.pdf","rb"))
page1 = pr.pages[0]
# extend the paper to letter size
mbox = page1.mediaBox
newh = round(float(mbox[2])*LETTERSIZE[1]/LETTERSIZE[0])
deltaH = newh - float(mbox[3])
page1.mediaBox = PyPDF2.generic.RectangleObject([0,-1*deltaH,mbox[2],mbox[3]])
# write it
wr = PyPDF2.PdfFileWriter()
wr.addPage(page1)
wr.write(open("my_letter_size_pdf.pdf","wb"))
{% endhighlight %}

