# Using GitHub/MyBinder to share Jupyter notebooks (R and Python)

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

## Post notebook to GitHub

Uh oh, that probably didn't work.

## Add extra bits to GitHub repo for Binder to work

### Add data directly to repo

### Export conda details to environment.yml

## Binder

### Binder explanation

## Binder R example

## Other links

[Spacy 101](https://spacy.io/usage/spacy-101)
