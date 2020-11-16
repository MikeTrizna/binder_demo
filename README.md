# Using GitHub/MyBinder to share Jupyter notebooks (R and Python)

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/MikeTrizna/binder_demo/HEAD)

Today's demo will attempt to show how MyBinder is a more powerful tool for sharing Jupyter notebooks. They allow you to share an interactive version of your notebook, along with the associated data. As an added bonus, the environment specification files needed to make MyBinder run properly are best practice for sharing notebooks and analyses with collaborators.

## Create a GitHub repository to track progress and store files

Before getting started, log into Github.com and create a new repository with a README. Then use the [GitHub desktop app](https://desktop.github.com/) to clone the new repository to your computer.

## Build a local Jupyter notebook that uses plotly

### Start by creating a blank conda environment

In a terminal window: 

`conda create -n binder_demo notebook pandas`

This creates a new conda environment named binder_demo, and installs the Python packages notebook (lightweight version of jupyter) and pandas.

### Launch jupyter

Make sure to activate this new environment

`conda activate binder_demo`

And then launch jupyter:

`jupyter notebook`

This will open a browser window in the Jupyter interface. Browse through the file system to where you cloned the GitHub repo, and then create a new Jupyter notebook there.

### Overview of Jupyter

[Jupyter Notebooks](https://jupyter.org/) are a framework for [literate programming](https://en.wikipedia.org/wiki/Literate_programming), where you can combine descriptive text and figures alongside code.

One cool example of this is the ability to embed YouTube videos:

```python
from IPython.display import YouTubeVideo
YouTubeVideo('hVimVzgtD6w')
```

This command embeds the YouTube video of [Hans Rosling's TED talk from 2007](https://www.youtube.com/watch?v=hVimVzgtD6w). The talk uses UN data to show that the world is getting better, and that very smart people do not realize this. Also check out his book [Factfulness](https://smile.amazon.com/Factfulness-Reasons-World-Things-Better-ebook/dp/B0756J1LLV/), which expands on this talk.

We will be using the "gapminder" dataset to recreate a figure from his talk. Download it from here: https://www.dropbox.com/s/46xl8fpi0qa3pd4/gapminder_data.tsv?dl=0. 

### Use pandas to load gapminder

```python
gap_df = pd.read_csv('gapminder_data.tsv', sep='\t')
gap_df.head()
```

### Open Jupyter terminal to install plotly-express

In the Python Data Carpentries workshops (depending on which version you took), we covered using matplotlib or plotnine for data visualization. These 2 packages create charts as images, but in this tutorial we will use a Python library called plotly-express (https://plot.ly/python/plotly-express/) to produce interactive charts.

Since we only installed jupyter and pandas in our previous conda environment step, we will need to separately install plotly-express. 

Go back to the Jupyter "home page", and under the New dropdown, select Terminal.

First, activate the environment:

```conda activate binder_demo```

Then install plotly-express:

```conda install -c conda-forge plotly_express```

### Use plotly-express to create hover plot of single year

```python
fig = px.scatter(gap_df[gap_df['year'] == 2007], 
                 x="gdp_per_capita", y="life_expectancy", 
                 size="population", color="continent",
                 hover_name="country", 
                 log_x=True, size_max=60)
fig.show()
```

### Use plotly-express to create animated plot of all years in dataset

```python

fig = px.scatter(gap_df, 
                 x="gdp_per_capita", y="life_expectancy", 
                 size="population", color="continent",
                 hover_name="country", 
                 log_x=True, size_max=45, range_x=[100,100000], range_y=[25,90],
                 animation_frame="year", animation_group="country")
fig.show()
```

## Commit notebook to GitHub

Don't need to commit .ipynb_checkpoint files/folders.

By default, GitHub renders notebooks, but it doesn't always render all elements properly. Videos don't embed, interactive plotly plots don't display.

If you share the file, you need to tell the recipient which packages to install and provide them the data
- add data to repo directory and make link relative

## Add data to GitHub repo

Change read_csv function to read from a relative path

Commit data file to GitHub repo

## Add details of conda environment to GitHub repo

In order for a collaborator to run the notebook (or for Binder to set up the environment properly), they need to have the correct packages installed in their conda environment.

### Export conda details to environment.yml

In terminal/command line (either local machine or Jupyter):

`conda env export`: shows all packages installed and their exact version. Too detailed!

`conda env export --from-history`: shows all packages you explicitly told conda to install

`conda env export --from-history > environment.yml`: saves conda environment to a YAML file, environment.yml

In text editor, edit environment.yml. Add `- plotly` as a second channel below `- default`.

Commit environment.yml to GitHub repo. Can commit it in its own directory, `binder`.

### Create conda environment from YAML file

`conda env create -f environment.yml`: creates a new conda environment based on the specifications from environment.yml. Binder will automatically take care of this, but a user could manually create a new environment this way.

## Binder

mybinder.org

Takes a GitHub repository as input. As long as repo contains certain files, Binder builds and hosts an entire Jupyter environment that anyone can use.

Binder provides code that you can include in your repository's README.md file, to direct people to the Binder instance.

Takes a while to build the first time, but once built, it's cached for a while. (Maybe a month?)

Once in the Jupyter browser, can use Upload button to upload your own data to Binder instance.

Limited to 1 GB of memory, so not a good tool for memory-intensive processes.

Size of data file is limited by the size of data file you can store in a GitHub repo.

There are ways to run R Shiny or OpenRefine through Binder environments.

## Other languages

Can use other languages in Jupyter notebook besides Python. Specifically, you can set up notebook to run R code.

conda has provided compiled versions of other libraries (r-dplyr, r-ggplot2). These can be imported into the Jupyter notebook, and can be run with an R kernel.

When running these, libraries can just be imported in the notebook normally (`library(dplyr)`, `library(ggplot2)`)

`ggplotly()`: function that wraps a ggplot-created graph, adds plotly interactivity

[Spacy 101](https://spacy.io/usage/spacy-101)
