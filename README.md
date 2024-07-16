### A Website with Personalized NBA Statsitics 


SliderStats is the all-in-one website that allows users to compare an unlimited number of stats between players. Users can weigh each stat, providing a completely customizable and more importantly, an objective approach to comparing performance metrics. Any basketball fan, ranging from the average casual to full-time ESPN analyst can benefit from SliderStats.
<br></br>
Unlike other websites for basketball statistics that only show raw stats, SliderStats uses a weighted sum ranking to provide further insight into multiple stats between players within a season. 

Visit our website: <b><a href="https://sliderstats.streamlit.app/" target="_blank">sliderstats.streamlit.app</a></b>

# Features

## Multitude of Statstics 

- **Per Game Stats:** Analyze player and team performance on a per-game basis.
- **Total Stats:** View cumulative statistics for players and teams.
- **Head-to-Head Comparison:** Compare stats between two players or teams.
- **Clutch Stats:** Examine performance during crucial moments of the game for specific players or teams.
- **Playoff Stats:** Focus on player performance during the playoffs.

## Trademark Sliders

- **Interactive Sliders:** Use sliders to filter and explore data interactively.

<img width="990" alt="image" src="https://github.com/AggieSportsAnalytics/NBASliderStats/blob/fb976cf6f3b769d093f4f8916aa507133c3da04c/images/seven_stats.png">

## Player Rankings

After filtering and adjusting sliders, see the final rankings of players/teams and their stats.

## Filtering

- **Year:** Filter ranking by year.
- **Position:** Filter ranking by position.
- **Statstics:** Choose which statistics to use.
- **Range of Stats:** Specify the range for each of your stats.

<img width="990" alt="image" src="https://github.com/AggieSportsAnalytics/scoreboard/blob/fb976cf6f3b769d093f4f8916aa507133c3da04c/images/hardware.png">

# Code

For our data, we use an API from github called nba_api which provides continuously updated data from the NBA's most current games and even games from decades before.

We use a variety of Python libraries, such as Pandas and Streamlit, to format and display the statsitics. Below is a snippet of the code used to filter statstics. 

for stat in selected_stats_per_game:
        sliders[stat] = st.sidebar.slider(f'Importance of {stat}', -1.0, 1.0, 0.5, 0.01)
    st.sidebar.title("Range of Stats")
    for stat in selected_stats_per_game:
        if stat in filtered_data_perGame:
            sliders_filter[stat] = st.sidebar.slider(f'Range of {stat}',
                                    float(filtered_data_perGame[stat][0]),
                                    float(filtered_data_perGame[stat][1]),
                                    (float(filtered_data_perGame[stat][0]),
                                    float(filtered_data_perGame[stat][1])))




    # Exclude all of the rows that are not in the player's range
    filtered_df = season_perGame_active_players.copy()
    for col in season_perGame_active_players.columns[1:]:
        if col in sliders_filter:
            filtered_df = filtered_df[filtered_df[col].between(*sliders_filter[col])]

    season_perGame_active_players = filtered_df

    # Filter the DataFrame based on selected stats
    filtered_data = season_perGame_active_players[selected_stats_per_game]


    # Calculate weighted sum based on user-defined importance
    normalized_dataPG = (filtered_data - filtered_data.min()) / (filtered_data.max() - filtered_data.min())
    # Calculate weighted sum based on user-defined importance
    weighted_totalsPG = normalized_dataPG * pd.Series(sliders)

    # Rounding for all per game stats
    season_perGame_active_players[columns_to_keepPG] = season_perGame_active_players[columns_to_keepPG].round(decimals=2)

    # Calculate the total weighted sum for each player
    season_perGame_active_players['Weighted_Sum'] = weighted_totalsPG.sum(axis=1)
    # Display sorted players based on weighted sum
    sorted_playersPG = season_perGame_active_players.sort_values(by=['Weighted_Sum', 'PLAYER_NAME'], ascending=[False, True])

    if drop_unused_stats:
        sorted_playersPG = pd.concat([sorted_playersPG['PLAYER_NAME'], sorted_playersPG[selected_stats_per_game], sorted_playersPG['Weighted_Sum']], axis = 1)

    st.write("Season Per Game Stats (Regular Season) for Active NBA Players:")
    st.write(sorted_playersPG)

# Technologies

- Python
- Pandas  
- Streamlit
- nba_api

# Contributors

Below are the key roles and team members involved in this project:  

- **Project Managers**: Juan Diaz and Jason Yang
- **Project Members**: Alex Qi and Ryan Uyeki
- **Media Members**: Ravinit Chand and Yaelle Kretchmer
- **Business Member(s)**: Aashritha Javvaji
