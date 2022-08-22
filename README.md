# Project-ppt
Project Presentation  in Data Analytics
for mathematical computation
import numpy as np
import pandas as pd
import scipy.stats as stats
#for data visualization
import seaborn as sns
import matplotlib.pyplot as plt
import plotly
import plotly.express as px
from matplotlib.pyplot import figure
import plotly.graph_objects as go
import plotly.figure_factory as ff
% matplotlib inline
df = pd.read_csv("/content/disney_plus_titles.csv", encoding='latin-1')
df.head()
disney_movies = df[df.type == 'Movie']
disney_shows = df[df.type == 'TV Show']
df["date_added"] = pd.to_datetime(df['date_added'])
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month
df['season_count'] = df.apply(lambda x : x['duration'].split(" ")[0] if "Season" in x['duration'] else "", axis = 1)
df['duration'] = df.apply(lambda x : x['duration'].split(" ")[0] if "Season" not in x['duration'] else "", axis = 1)
col = "release_year"
dm = disney_movies[col].value_counts().reset_index()
dm = dm.rename(columns = {col : "count", "index": col})
dm['percent'] = dm['count'].apply(lambda x : 100*x/sum(dm['count']))
dm = dm.sort_values(col)
fig1 = go.Bar(x = dm[col], y=dm['count'], name='Movies', marker=dict(color='red'))
data = [fig1]
layout = go.Layout(title='Movies content added on disney', legend=dict(x=0.1, y=1.1, orientation='h'))
fig = go.Figure(data, layout=layout)
fig.show()
col = "release_year"
ds = disney_shows[col].value_counts().reset_index()
ds = ds.rename(columns = {col : "count", "index": col})
ds['percent'] = ds['count'].apply(lambda x : 100*x/sum(ds['count']))
ds = ds.sort_values(col)
fig2 = go.Bar(x = ds[col], y=ds['count'], name='TV Shows', marker=dict(color='red'))
data = [fig2]
layout = go.Layout(title='TV Shows content added on disney', legend=dict(x=0.1, y=1.1, orientation='h'))
fig = go.Figure(data, layout=layout)
fig.show()
col = "year_added"
vc1 = disney_shows[col].value_counts().reset_index()
vc1 = vc1.rename(columns = {col : "count", "index" : col})
vc1['percent'] = vc1['count'].apply(lambda x : 100*x/sum(vc1['count']))
vc1 = vc1.sort_values(col)
vc2 = disney_movies[col].value_counts().reset_index()
vc2 = vc2.rename(columns = {col : "count", "index" : col})
vc2['percent'] = vc2['count'].apply(lambda x : 100*x/sum(vc2['count']))
vc2 = vc2.sort_values(col)
trace1 = go.Bar(x=vc1[col], y=vc1["count"], name="TV Shows", marker=dict(color="#a678de"))
trace2 = go.Bar(x=vc2[col], y=vc2["count"], name="Movies", marker=dict(color="#6ad49b"))
data = [trace1, trace2]
layout = go.Layout(title="Content added over the years on Disney+", legend=dict(x=0.1, y=1.1, orientation="h"))
fig = go.Figure(data, layout=layout)
fig.show()
disney_movies = df[df.type == 'Movie']
disney_shows = df[df.type == 'TV Show']
import plotly.figure_factory as ff
x1 = disney_movies['duration'].fillna(0.0).astype(float)
fig = ff.create_distplot([x1], ['a'], bin_size=0.7, curve_type='normal', colors=["red"])
fig.update_layout(title_text='Distplot with Normal Distribution')
fig.show()
