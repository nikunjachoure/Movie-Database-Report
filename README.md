
# Movie Database Report

### Dashboard Link : https://app.powerbi.com/view?r=eyJrIjoiYWVkMDAzNjYtZmUzYy00Nzg3LWJiYWYtNWRlNjc2NGIzMGRlIiwidCI6ImVlMmQ2ZDcyLTk1MzUtNDI0Mi1hMDc3LWFjZjE4NTc4MmY5YiIsImMiOjF9

## Vision


I have a deep appreciation for the world of cinema and often find myself facing the dilemma of choosing the perfect movie to watch. In an effort to streamline this decision-making process, I meticulously explore online reviews, recommendations, and IMDB ratings before settling on a film. Furthermore, I ponder the reliability of Netflix's suggestions in our contemporary movie-watching landscape.(There, I said it!)

To address this challenge, I undertook the task of creating a comprehensive movie database report using PowerBI. The dataset encompasses a diverse range of films dating from 1916 to 2016, allowing users to tailor their movie selections based on various criteria such as genres, actors, directors, and IMDB ratings. This dynamic dashboard provides users with the flexibility to make informed choices, transcending the conventional approach to movie selection.

Beyond movie recommendations, I delved into a detailed analysis of how different directors, actors, and movie genres have evolved and performed over the years. The resulting insights offer a fascinating glimpse into the trends and patterns that have shaped the cinematic landscape.

This PowerBI dashboard promises to revolutionize the way we engage with movies, providing a data-driven approach to curating an enriching cinematic journey.


Dataset Link - https://github.com/kishan0725/AJAX-Movie-Recommendation-System-with-Sentiment-Analysis/blob/master/datasets/movie_metadata.csv



## Steps followed 

### Dataset Overview and Data Preprocessing

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & create a duplicate of our source data and name it Movie_Data. This was done to preserve my original data (Movie_source)
- Step 3 : Removed unnecessary columns like director_facebook_likes, actor1_facebook_likes, etc.This step depends on the data you want to use in the dashboard for analysis.
- Step 4 : Renamed few columns like 'num_critic_for_review' as 'Critic Reviews'. This step can be performed as per personal preferences.
- Step 5 : Created a new custom column 'Profit %' using the below formula
```
[Box Office Revenue] - [Movie Budget] / [Movie Budget]
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Then changed type of this column to percentage. 

- Step 6 : Cobmine actor_1_name, actor_2_name, and actor_3_name columns into one column named "Cast" by using concatenate function

```
Table.AddColumn(#"Changed Type1", "Cast", each Text.Combine({[actor_1_name], " , ", [actor_2_name], " , ", [actor_3_name]}))
```

- Step 7 : Create an Index column (from 1) by choosing index column from Add Column tab. 
- Step 8 : Create a duplicate of Movie_Data after performing Steps 3 to Step 7 and name it Actor_Data
- Step 9 : Keep only Index, actor_1_name, actor_2_name, actor_3_name delete rest of the columns from Actor_Data
- Step 11 : Select all columns and use Unpivot columns option in Tranform tab to map the index to each actor. Now we have only three columns. Rename the value column as Actors and delete the attribute column. Remove all emplty columns from Actor column
- Step 12 : Perform Steps 7 to Step 9 for Genre column.
- Step 13 : Split the Genre column by using delimiter "|". Then perform Step 11 to map multiple genres to index.

### We have our data ready. We will now review the table relationships. 

- Step 14 : Open Model view tab and make sure that your three tables (Movie_Data, Actor_Data, Genre_Data) have active bidirectional relationship on index column.

### Let's move on to creating the required measures. 

- Step 15 : Create following Measures

| Measure Name | DAX Formula |
| --------     | -------     |
| Total Movies      | ```Total Movies = COUNT('Movie Metadata Main'[Index])```        |
| Total Critic Reviews     | ```Critic Review Count = SUM('Movie Metadata Main'[Critic Reviews Count]) ```         |
| Total User Reviews     | ```User Review Count = SUM('Movie Metadata Main'[Number of Reviews])```         |
| Average Runtime (Mins)        | ```Average Movie Runtime = AVERAGE('Movie Metadata Main'[Movie Duration (Min)]) ```       |
| Average IMDB Rating        | ```Average IMDB Rating = AVERAGE('Movie Metadata Main'[IMDB Rating])```       |
| Average Revenue        | ```Average Revenue = AVERAGE('Movie Metadata Main'[Box Office Revenue])```        |
| Average Budget        | ```Average Budget = AVERAGE('Movie Metadata Main'[Movie Budget]) ```        |

### Creating the charts

- Step 16 : Create the landing/summary page
    -   Create cards for all above mentioned measures (get the reference from the summary page snapshot below)
    - Add a filled-map visualization to provide total movies by country
    - Add a donut-chart visualization to show Color & Black-White Movies
    - Add a stacked column-chart for Movies by Rating
    - Add a stacked bar-chart to show top 5 most user reviewed movies by IMDB Rating. Use Top N filter on "Movie Title" field using "Total User Reviews" measure
    - Add a area-chart to show total movies by release year and by color.

![Summary_Page](https://raw.githubusercontent.com/nikunjachoure/movie_database_report/main/summary.png)
<p align="center">Snapshot of Summary Page</p>

- Step 17 : Create the Directors page
    - Add the visualizations based on the Director's snapshot below
    - Create two slicers for "Country" and "Language" to filter the charts according to these slicers.
![Summary_Page](https://github.com/nikunjachoure/movie_database_report/blob/main/directors.png?raw=true)
<p align="center">Snapshot of Director Page</p>

- Step 18 : Create Actors page
    - Create 4 visualizations along with 3 slicers for "Language", "Actor" and "Country" as shown in the snapshot below
![Summary_Page](https://github.com/nikunjachoure/Movie-Database-Report/blob/main/actors.png?raw=true)
<p align="center">Snapshot of Actor Page</p>

- Step 19 : Create the Genre page as shown in the snapshot below
![Summary_Page](https://github.com/nikunjachoure/movie_database_report/blob/main/genre.png?raw=true)
<p align="center">Snapshot of Genre Page</p>

### Creating clickable buttons
- Step 20 : Add a navigation button ("Right arrow"), which when clicked will take you to the next consecutive page. Add a hovering effects, action and appropriate tooltip.
- Step 21 : Create a custom Control Panel
    - Create a custom icon using powerpoint (or you can donwload any relevant icon)
    - Add a rectangle and format it to match the theme
    - Add 5 buttons (4 for each page and one to close the Control Panel)
    - Add 5 slicers along with the buttons
    - Add text ("Control Panel") to the rectangular shape
    - Select Bookmark and Selection from the View tab in the toolbar. 
    - Create three Bookmarks for each page (one to view control, one to hide controls and one for resetting the controls)
    - Make sure the data is not selected in Bookmarks, in-order to retain slicers information to the correponding visualizations
![Summary_Page](https://github.com/nikunjachoure/movie_database_report/blob/main/control_panel.png?raw=true)
<p align="center">Snapshot of Control Panel</p>

- Step 22 : Create a Reset-filter button for reverting to the default state of all visualizations. In this case, make sure the data is enabled in the Bookmarks.


