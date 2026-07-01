# Project Overview

## Problem Statement
Choosing a film is often difficult when the available options are unfamiliar. Whether browsing a local cinema's weekly programme, a streaming watchlist, or a film festival schedule, viewers typically have little information beyond a title, synopsis, trailer, or a single IMDb rating.

These sources provide limited insight into the people behind a film and their previous work, making it harder to judge whether a film is likely to match a viewer's interests.

## Solution
This project extends a screening programme with historical IMDb data to provide additional context for decision-making. Instead of evaluating only the films currently showing, users can explore the track record of the directors, writers, actors, producers, composers, cinematographers, editors, and other contributors behind each film.

The dashboard supports four interactive views:

### 1. Screening Programme Overview
- Compare films in the selected programme using IMDb Rating and IMDb Votes. 
- Compare its contributors based on the average IMDb rating and total IMDb votes of their previous work.

 URL: https://public.tableau.com/app/profile/claudia.werner/viz/Festivalguide/FestivalBubble

### 3. Career Timeline Individual Contributor
Explore an individual contributor's complete filmography and career progression over time.

### 4. Cast & Crew Explorer
View and compare the historical filmographies of the entire creative team behind a selected film.

And this will produce a flow chart:

```mermaid
flowchart LR
  subgraph "IMDb raw sources"
    A1["title.basics.tsv.gz"]
    A2["name.basics.tsv.gz"]
    A3["title.ratings.tsv.gz"]
    A4["title.principals.tsv.gz"]
    A5["title.crew.tsv.gz"]
  end

  subgraph "Festival CSV archive"
    B1["festival CSV files"]
  end

subgraph "Festival CSV archive"
        U1["ratings.csv"]
  end
  subgraph "dwh_titles__details.ipynb"
    D1["dwh_titles__details.csv"]
    D2["dwh_principals__details.csv"]
    D3["dwh_titles__ratings.csv"]
  end

  subgraph "dwh_xref_titles_principals__filtered.ipynb"
    D4["dwh_xref_titles_principals__filtered.csv"]
  end

  subgraph "dwh_titles__editor_lists.ipynb"
    D5["dwh_titles__editor_lists.csv"]
    D6["dwh_editor_lists__pivoted.csv"]
  end

 subgraph "mrt_titles__editor_lists.ipynb"
    D12["mrt_titles__editor_lists.csv"]
  end

  subgraph "mrt_xref_title_principals__enriched.ipynb"
    D7["mrt_xref_titles_principals__enriched.csv"]

  end

  subgraph "mrt_xref_title_principals__rating_profiles.ipynb"
      D8["Xfst"]
         D10["mrt_principals__aggregated.csv"]
         D9["mrt_titles__per_rating_category.csv"]
         D11["mrt_principals__per_rating_category.csv"]
    end
  
  A1 --> D1
  A2 --> D2
  A3 --> D3
  A4 --> D4
  A5 --> D4

  B1 --> D5

  D1 --> D7
  D2 --> D7
  D3 --> D7
  D4 --> D7
  U1 --> D7

  D7 --> D8
  D5 --> D8

  D8 --> D9
  D8 --> D10
  D8 --> D11

  D5 --> D12

  D7  --> Tableau_loved_movie
  D7  --> Tableau_underrated
  D12 --> Tableau_underrated

  D9  --> Tableau_festival_guide
  D11  --> Tableau_festival_guide

  subgraph "xto, has to be same folder"
     Tableau_festival_guide
     Tableau_loved_movie
     Tableau_underrated
    end
```

Loved that movie? Wonder whether cast & crew has teamed up before? 
See their entire filmgraphy in one glance, based on data extracted from IMDb (Python) and loaded into a dashboard (Tableau Public).

https://public.tableau.com/app/profile/claudia.werner/viz/LovedthatMovieCastCrewFilmography/CastCrew
