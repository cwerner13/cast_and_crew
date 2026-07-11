# Project Overview

## Problem Statement
Choosing a film is often difficult when the available options are unfamiliar. Whether browsing a local cinema's weekly programme, a streaming watchlist, or a film festival schedule, viewers typically have little information beyond a title, synopsis, trailer, or a single IMDb rating.

These sources provide limited insight into the people behind a film and their previous work, making it harder to judge whether a film is likely to match a viewer's interests.

## Solution
This project extends a screening programme with historical IMDb data to provide additional context for decision-making. Instead of evaluating only the films currently showing, users can explore the track record of the directors, writers, actors, producers, composers, cinematographers, editors, and other contributors behind each film.

Tableau dashboards are linked for interactive views:

### 1. Festival Guide / Screening Programme Overview
- Compare films in the selected programme using IMDb Rating and IMDb Votes. 
- Compare its contributors based on the average IMDb rating and total IMDb votes of their previous work.

with option to select film's imdb website or  Cast & Crew Filmography<br> 
URL: https://public.tableau.com/app/profile/claudia.werner/viz/Festivalguide/FestivalBubble

### 2. Cast & Crew Filmography

 -  #### a. Individual Film: Cast & Crew Explorer
  View and compare the historical filmographies of the entire creative team behind a selected film.<br>
  URL: https://public.tableau.com/app/profile/claudia.werner/viz/LovedthatMovieCastCrewFilmography/CastCrew?Tconst_sel_p=tt10370710

 -  #### b. Individual Contributor: Carrer Timeline
  Explore an individual contributor's complete filmography and career progression over time. <br>
  URL: https://public.tableau.com/app/profile/claudia.werner/viz/LovedthatMovieCastCrewFilmography/CastCrew?Tconst_sel_p=tt10370710

### 3. Most Underrated Movies
Individual User Rating vs IMDb rating, ranked by rating discrepancy
URL: in progress

And this will produce a flow chart:
```mermaid
 flowchart LR

classDef default fill:#ffffff,stroke:#000000,color:#000000;
classDef dashed fill:none,stroke:#000000,stroke-width:1px,stroke-dasharray: 5 5;
 
%%{init: {
  "theme": "base",
  "themeVariables": {
    "clusterBkg": "#c4c0c033",
    "clusterBorder": "grey",
    "primaryColor": "#ffffff",
    "primaryBorderColor": "#000000",
    "primaryTextColor": "#000000"
  }
}}%%

subgraph "sources: ...\rpi\ENV\interfaces\"
    subgraph "rpi10008_imdb__editor_lists\import\archive"
              rpi10008_sg1["www.imdb.com/user/.../lists"]
    end
    
    subgraph "rpi10009_imdb__user_ratings\import\archive"
              rpi10009_sg1["www.imdb.com/user/.../ratings"]
    end

    subgraph "IMDb raw sources"
          
             A2["name.basics.tsv.gz"]
             A1["title.basics.tsv.gz"]
             A3["title.ratings.tsv.gz"]
             A4["title.principals.tsv.gz"]
             A5["title.crew.tsv.gz"]
    end
 end

  %%% dwh
  subgraph "dwh ...\rpi\ENV\transformations\"
  subgraph "dwh_titles__details.ipynb"
    D1["dwh_titles__details.csv"]
    D3["dwh_titles__ratings.csv"]
  end

    subgraph "dwh_principals__details.ipynb"
    D2["dwh_principals__details.csv"]
  end


  subgraph "dwh_xref_titles_principals__filtered.ipynb"
    D4["dwh_xref_titles_principals__filtered.csv"]
  end



  subgraph "dwh_titles__user_profile_ratings.ipynb"
    rpi10009_sg3["ratings.csv"]
  end
  subgraph "dwh_titles__editor_lists.ipynb"
    D5["dwh_titles__editor_lists.csv"]
    D6["dwh_editor_lists__pivoted.csv"]
  end
end
 
  %%% mart
  subgraph "mrt/xto   ...\rpi\ENV\interfaces\rpi20003_twb__cast_and_crew\export\current"

    subgraph "mrt_titles__editor_lists.ipynb"
              D12["mrt_titles__editor_lists.csv"]
    end

    subgraph "mrt_xref_title_principals__enriched.ipynb"
              D7["mrt_xref_titles_principals__enriched.csv"]
    end


    

    subgraph "mrt_xref_title_principals__rating_profiles.ipynb"
             D8["Xfst"]:::dashed
             D10["mrt_principals__aggregated.csv"]
             D9["mrt_titles__per_rating_category.csv"]
            D11["mrt_principals__per_rating_category.csv"]
    end
  end 
 

  

  %%% connections
  A1 --> D1
  A2 --> D2
  A3 --> D3
  A4 --> D4
  A5 --> D4

   
 
  rpi10008_sg1   --> D5
  rpi10009_sg1   --> rpi10009_sg3 --> D7



  D1 --> D7
  D2 --> D7
  D3 --> D7
  D4 --> D7

  D7 --> D8
  D5 --> D8

  D8 --> D9
  D8 --> D10
  D8 --> D11

  D5 --> D12

 
  D7  --> Most_Underrated_Movies
  D12 --> Most_Underrated_Movies
  D7  --> Cast_and_Crew_Filmography
  D9  --> Festivalguide
  D11  --> Festivalguide

 
  subgraph "https://public.tableau.com/" 
     Festivalguide
     Cast_and_Crew_Filmography
     Most_Underrated_Movies
    end
```

Loved that movie? Wonder whether cast & crew has teamed up before? 
See their entire filmgraphy in one glance, based on data extracted from IMDb (Python) and loaded into a dashboard (Tableau Public).

https://public.tableau.com/app/profile/claudia.werner/viz/LovedthatMovieCastCrewFilmography/CastCrew
