# Setting up Jupyter Notebook
### Notes about the configuration of Jupyter Notebook in Linux

## choose browser 
* to choose which browser to launch when runnning the the notebook in console. For chromium-browser write in an active environment:
>BROWSER=chromium-browser jupyter notebook

After that a set of links appear. One of them should be run in chromiun 

---
## Favorite Commands

* **Esc** will take you into command mode where you can navigate around your notebook with arrow keys.
 * **F** find and replace
 * **Esc + F** Find and replace on your code but not the outputs.
 * **ctrl+/** to comment a block text
 * **P** to see the pallete launcher
 * **%%time** to see the time of cell running 
 * **Shift + Tab** will show you the Docstring (documentation) for the the object you have just typed in a code cell – you can keep pressing this short cut to cycle through a few modes of documentation.
* **Ctrl + Shift + -** will split the current cell into two from where your cursor is.
* **Shift+ Enter** to comment all the selected text

* **>** before a text to make an indented quotate 
	* >la forma de 


*  **While in command mode:**
   *    **A** to insert a new cell above the current cell
   *    **B** to insert a new cell below
   *    **X** to delete a cell 
   *    **M** to change the current cell to Markdown, Y to change it back to code
   *    **D + D** (press the key twice) to delete the current cell
   *    **Enter** will take you from command mode back into edit mode for the given cell.

   
*  **Select Multiple Cells:**
   *    **Shift + J or Shift + Down** selects the next sell in a downwards direction. 
   *    **Shift + K or Shift + Up** selects sells in an upwards direction.
	 *    **Once cells are selected**
           * **delete / copy / cut / paste / run** them as a batch. This is helpful when you need to move parts of a notebook.
            * **Shift + M** to merge multiple cells.

## Plotting in notebooks
https://www.dataquest.io/blog/jupyter-notebook-tips-tricks-shortcuts/

* **matplotlib** (the de-facto standard), activated with **%matplotlib inline**  – Dataquest Matplotlib Tutorial: https://www.dataquest.io/blog/matplotlib-tutorial/
  *   %matplotlib notebook provides interactivity but can be a little slow, since rendering is done server-side.
* **Seaborn** is built over Matplotlib and makes building more attractive plots easier. Just by importing Seaborn, your matplotlib plots are made ‘prettier’ without any code modification.
    mpld3 provides alternative renderer (using d3) for matplotlib code. Quite nice, though incomplete.
* **bokeh** is a better option for building interactive plots.
* **plot.ly** can generate nice plots – this used to be a paid service only but was recently open sourced.
* **Altair** is a relatively new declarative visualization library for Python. It’s easy to use and makes great looking plots, however the ability to customize those plots is not nearly as powerful as in Matplotlib.


## Jupyter Tricks 
https://www.analyticsvidhya.com/blog/2020/04/10-productive-jupyter-notebook-hacks-tips-tricks/


1. You can write in only one cell and get several results
data.shape
data.head()
data.dtypes
data.info()

2. By naming the variable wrong at multiple places, correct them in one go using multicursor and **CTRL + Click** at the desired points!

3. **%who** is a useful magic command that returns the variables present in the global space along with its type.

4. **%history** provides a list of all the commands given in the notebook


    * **%history -n** print the line number of each command
    * **%history -o** print the command and output
    * **%history -t** print translated history as python command


5. jupyterthemes 

  *  Install jupyter-themes:
       *     using anaconda conda install -c conda-forge jupyterthemes
        * Check the list of themes – jt - l
        * Select a theme jt -t chesterish
        * To restore to default theme – jt -r
6. Change Cell Width: jupyter-themes provides a very simple way to alter cell width height and width:
    Change the theme, cell width, cell height **jt -t chesterish -cellw 100% lineh 170**
7. Share notebooks via Jupyter nbviewer
8. Nbconvert to Present your Notebook as Slides 
9. %prun – Run Python Profiler

Jupyter notebook provides a simple wrapper to run Python Profiler – %prun. A profiler helps you analyze your code so that you can optimize the pain points. It provides you with the number of function calls and the execution time. Other information provided is:

    ncalls – number of calls
    tottime – The total time spent in the given function (excluding time made in calls to sub-functions)
    percall – the quotient of tottime divided by ncalls
    cumtime –  the total time spent in this and all subfunctions (from invocation till exit)
    percall – the quotient of cumtime divided by primitive calls
10.  %%heat – Heatmap over Code

This last hack is one of the true gems out there. It’s one of the easiest ways to visualize the profiling of your code. By using %%heat, you’ll get an output of your code in the form of a heatmap. This will help you recognize the most time-consuming statements and help you optimize it.

Let’s checkout the steps in your Jupyter notebook:

    !pip install py-heat-magic
    To load the heat as a magic function in your notebook – %load_ext heat
    Use %%heat in your notebook cell for which you want to get the heatmap


---
### Tutorials & Extensions for Data Sscience 
**to develop**
- [10-useful-jupyter-notebook-extensions-for-a-data-scientist](https://towardsdatascience.com/10-useful-jupyter-notebook-extensions-for-a-data-scientist-bd4cb472c25e)
- [jason+pandas](https://www.dataquest.io/blog/python-json-tutorial/)
- [dataframe-gui-overview](https://pbpython.com/dataframe-gui-overview.html)
