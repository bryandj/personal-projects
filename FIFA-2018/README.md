
# <center>FIFA 18 Player Dataset Exploration</center>
![fifa](images/fifa.jpg)

# Table of contents
1. [Initial Exploration](#initial)
2. [Positions](#positions)
3. [Height and Weight](#height)
4. [Top 5 Leagues](#leagues)
5. [England vs. Germany PKs](#PKs)
6. [Southpaws](#southpaws)
6. [Similarity](#similarity)

# Initial Exploration <a name="initial"></a>
The FIFA video game franchise is a worldwide phenomenon, accounting for [roughly 40%](https://www.forbes.com/sites/greatspeculations/2017/10/10/fifa-remains-eas-bread-and-butter/#4b55f3ef2140) of Electronic Arts' yearly revenue. FIFA 18, released in Nov. 2017, sold 5.9 million units in its first week alone, and surpassed 10 million units by the end of the year. In order to meet the demands of soccer fanatics across the globe, EA strives for accuracy, and as such, each FIFA iteration includes an enormous database of player statistics. In this notebook, we explore the FIFA 18 database (retrieved from [Kaggle](https://www.kaggle.com/thec03u5/fifa-18-demo-player-dataset)).

Let's begin by getting an idea at the scope of the dataset.


```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
import numpy as np

plt.style.use('seaborn')

fifa = pd.read_csv('complete.csv')
```


```python
print('Players:', '\t', fifa.shape[0], '\n',
      'Nationalities:', '\t', fifa.groupby('nationality')['ID'].count().size, '\n',
      'Clubs:', '\t\t', fifa.groupby('club')['ID'].count().size, '\n',
      'Leagues:', '\t', fifa.groupby('league')['ID'].count().size, sep='')
```

    Players:	17994
    Nationalities:	164
    Clubs:		647
    Leagues:	41


Wow, that is a lot of players and clubs. Now let's look at what data the dataframe actually contains. Note that the players in the dataframe are indexed in descending order based on the **overall** stat, which is commonly viewed as the most important/significant stat for a player.


```python
with pd.option_context('display.max_rows', None, 'display.max_columns', None):
    display(fifa.head())
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>name</th>
      <th>full_name</th>
      <th>club</th>
      <th>club_logo</th>
      <th>special</th>
      <th>age</th>
      <th>league</th>
      <th>birth_date</th>
      <th>height_cm</th>
      <th>weight_kg</th>
      <th>body_type</th>
      <th>real_face</th>
      <th>flag</th>
      <th>nationality</th>
      <th>photo</th>
      <th>eur_value</th>
      <th>eur_wage</th>
      <th>eur_release_clause</th>
      <th>overall</th>
      <th>potential</th>
      <th>pac</th>
      <th>sho</th>
      <th>pas</th>
      <th>dri</th>
      <th>def</th>
      <th>phy</th>
      <th>international_reputation</th>
      <th>skill_moves</th>
      <th>weak_foot</th>
      <th>work_rate_att</th>
      <th>work_rate_def</th>
      <th>preferred_foot</th>
      <th>crossing</th>
      <th>finishing</th>
      <th>heading_accuracy</th>
      <th>short_passing</th>
      <th>volleys</th>
      <th>dribbling</th>
      <th>curve</th>
      <th>free_kick_accuracy</th>
      <th>long_passing</th>
      <th>ball_control</th>
      <th>acceleration</th>
      <th>sprint_speed</th>
      <th>agility</th>
      <th>reactions</th>
      <th>balance</th>
      <th>shot_power</th>
      <th>jumping</th>
      <th>stamina</th>
      <th>strength</th>
      <th>long_shots</th>
      <th>aggression</th>
      <th>interceptions</th>
      <th>positioning</th>
      <th>vision</th>
      <th>penalties</th>
      <th>composure</th>
      <th>marking</th>
      <th>standing_tackle</th>
      <th>sliding_tackle</th>
      <th>gk_diving</th>
      <th>gk_handling</th>
      <th>gk_kicking</th>
      <th>gk_positioning</th>
      <th>gk_reflexes</th>
      <th>rs</th>
      <th>rw</th>
      <th>rf</th>
      <th>ram</th>
      <th>rcm</th>
      <th>rm</th>
      <th>rdm</th>
      <th>rcb</th>
      <th>rb</th>
      <th>rwb</th>
      <th>st</th>
      <th>lw</th>
      <th>cf</th>
      <th>cam</th>
      <th>cm</th>
      <th>lm</th>
      <th>cdm</th>
      <th>cb</th>
      <th>lb</th>
      <th>lwb</th>
      <th>ls</th>
      <th>lf</th>
      <th>lam</th>
      <th>lcm</th>
      <th>ldm</th>
      <th>lcb</th>
      <th>gk</th>
      <th>1_on_1_rush_trait</th>
      <th>acrobatic_clearance_trait</th>
      <th>argues_with_officials_trait</th>
      <th>avoids_using_weaker_foot_trait</th>
      <th>backs_into_player_trait</th>
      <th>bicycle_kicks_trait</th>
      <th>cautious_with_crosses_trait</th>
      <th>chip_shot_trait</th>
      <th>chipped_penalty_trait</th>
      <th>comes_for_crosses_trait</th>
      <th>corner_specialist_trait</th>
      <th>diver_trait</th>
      <th>dives_into_tackles_trait</th>
      <th>diving_header_trait</th>
      <th>driven_pass_trait</th>
      <th>early_crosser_trait</th>
      <th>fan's_favourite_trait</th>
      <th>fancy_flicks_trait</th>
      <th>finesse_shot_trait</th>
      <th>flair_trait</th>
      <th>flair_passes_trait</th>
      <th>gk_flat_kick_trait</th>
      <th>gk_long_throw_trait</th>
      <th>gk_up_for_corners_trait</th>
      <th>giant_throw_in_trait</th>
      <th>inflexible_trait</th>
      <th>injury_free_trait</th>
      <th>injury_prone_trait</th>
      <th>leadership_trait</th>
      <th>long_passer_trait</th>
      <th>long_shot_taker_trait</th>
      <th>long_throw_in_trait</th>
      <th>one_club_player_trait</th>
      <th>outside_foot_shot_trait</th>
      <th>playmaker_trait</th>
      <th>power_free_kick_trait</th>
      <th>power_header_trait</th>
      <th>puncher_trait</th>
      <th>rushes_out_of_goal_trait</th>
      <th>saves_with_feet_trait</th>
      <th>second_wind_trait</th>
      <th>selfish_trait</th>
      <th>skilled_dribbling_trait</th>
      <th>stutter_penalty_trait</th>
      <th>swerve_pass_trait</th>
      <th>takes_finesse_free_kicks_trait</th>
      <th>target_forward_trait</th>
      <th>team_player_trait</th>
      <th>technical_dribbler_trait</th>
      <th>tries_to_beat_defensive_line_trait</th>
      <th>poacher_speciality</th>
      <th>speedster_speciality</th>
      <th>aerial_threat_speciality</th>
      <th>dribbler_speciality</th>
      <th>playmaker_speciality</th>
      <th>engine_speciality</th>
      <th>distance_shooter_speciality</th>
      <th>crosser_speciality</th>
      <th>free_kick_specialist_speciality</th>
      <th>tackling_speciality</th>
      <th>tactician_speciality</th>
      <th>acrobat_speciality</th>
      <th>strength_speciality</th>
      <th>clinical_finisher_speciality</th>
      <th>prefers_rs</th>
      <th>prefers_rw</th>
      <th>prefers_rf</th>
      <th>prefers_ram</th>
      <th>prefers_rcm</th>
      <th>prefers_rm</th>
      <th>prefers_rdm</th>
      <th>prefers_rcb</th>
      <th>prefers_rb</th>
      <th>prefers_rwb</th>
      <th>prefers_st</th>
      <th>prefers_lw</th>
      <th>prefers_cf</th>
      <th>prefers_cam</th>
      <th>prefers_cm</th>
      <th>prefers_lm</th>
      <th>prefers_cdm</th>
      <th>prefers_cb</th>
      <th>prefers_lb</th>
      <th>prefers_lwb</th>
      <th>prefers_ls</th>
      <th>prefers_lf</th>
      <th>prefers_lam</th>
      <th>prefers_lcm</th>
      <th>prefers_ldm</th>
      <th>prefers_lcb</th>
      <th>prefers_gk</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20801</td>
      <td>Cristiano Ronaldo</td>
      <td>C. Ronaldo dos Santos Aveiro</td>
      <td>Real Madrid CF</td>
      <td>https://cdn.sofifa.org/18/teams/243.png</td>
      <td>2228</td>
      <td>32</td>
      <td>Spanish Primera División</td>
      <td>1985-02-05</td>
      <td>185.0</td>
      <td>80.0</td>
      <td>C. Ronaldo</td>
      <td>True</td>
      <td>https://cdn.sofifa.org/flags/38@3x.png</td>
      <td>Portugal</td>
      <td>https://cdn.sofifa.org/18/players/20801.png</td>
      <td>95500000.0</td>
      <td>565000.0</td>
      <td>195800000.0</td>
      <td>94</td>
      <td>94</td>
      <td>90</td>
      <td>93</td>
      <td>82</td>
      <td>90</td>
      <td>33</td>
      <td>80</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>High</td>
      <td>Low</td>
      <td>Right</td>
      <td>85</td>
      <td>94</td>
      <td>88</td>
      <td>83</td>
      <td>88</td>
      <td>91</td>
      <td>81</td>
      <td>76</td>
      <td>77</td>
      <td>93</td>
      <td>89</td>
      <td>91</td>
      <td>89</td>
      <td>96</td>
      <td>63</td>
      <td>94</td>
      <td>95</td>
      <td>92</td>
      <td>80</td>
      <td>92</td>
      <td>63</td>
      <td>29</td>
      <td>95</td>
      <td>85</td>
      <td>85</td>
      <td>95</td>
      <td>22</td>
      <td>31</td>
      <td>23</td>
      <td>7</td>
      <td>11</td>
      <td>15</td>
      <td>14</td>
      <td>11</td>
      <td>92.0</td>
      <td>91.0</td>
      <td>91.0</td>
      <td>89.0</td>
      <td>82.0</td>
      <td>89.0</td>
      <td>62.0</td>
      <td>53.0</td>
      <td>61.0</td>
      <td>66.0</td>
      <td>92.0</td>
      <td>91.0</td>
      <td>91.0</td>
      <td>89.0</td>
      <td>82.0</td>
      <td>89.0</td>
      <td>62.0</td>
      <td>53.0</td>
      <td>61.0</td>
      <td>66.0</td>
      <td>92.0</td>
      <td>91.0</td>
      <td>89.0</td>
      <td>82.0</td>
      <td>62.0</td>
      <td>53.0</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>158023</td>
      <td>L. Messi</td>
      <td>Lionel Messi</td>
      <td>FC Barcelona</td>
      <td>https://cdn.sofifa.org/18/teams/241.png</td>
      <td>2158</td>
      <td>30</td>
      <td>Spanish Primera División</td>
      <td>1987-06-24</td>
      <td>170.0</td>
      <td>72.0</td>
      <td>Messi</td>
      <td>True</td>
      <td>https://cdn.sofifa.org/flags/52@3x.png</td>
      <td>Argentina</td>
      <td>https://cdn.sofifa.org/18/players/158023.png</td>
      <td>105000000.0</td>
      <td>565000.0</td>
      <td>215300000.0</td>
      <td>93</td>
      <td>93</td>
      <td>89</td>
      <td>90</td>
      <td>86</td>
      <td>96</td>
      <td>26</td>
      <td>61</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>Medium</td>
      <td>Medium</td>
      <td>Left</td>
      <td>77</td>
      <td>95</td>
      <td>71</td>
      <td>88</td>
      <td>85</td>
      <td>97</td>
      <td>89</td>
      <td>90</td>
      <td>87</td>
      <td>95</td>
      <td>92</td>
      <td>87</td>
      <td>90</td>
      <td>95</td>
      <td>95</td>
      <td>85</td>
      <td>68</td>
      <td>73</td>
      <td>59</td>
      <td>88</td>
      <td>48</td>
      <td>22</td>
      <td>93</td>
      <td>90</td>
      <td>78</td>
      <td>96</td>
      <td>13</td>
      <td>28</td>
      <td>26</td>
      <td>6</td>
      <td>11</td>
      <td>15</td>
      <td>14</td>
      <td>8</td>
      <td>88.0</td>
      <td>91.0</td>
      <td>92.0</td>
      <td>92.0</td>
      <td>84.0</td>
      <td>90.0</td>
      <td>59.0</td>
      <td>45.0</td>
      <td>57.0</td>
      <td>62.0</td>
      <td>88.0</td>
      <td>91.0</td>
      <td>92.0</td>
      <td>92.0</td>
      <td>84.0</td>
      <td>90.0</td>
      <td>59.0</td>
      <td>45.0</td>
      <td>57.0</td>
      <td>62.0</td>
      <td>88.0</td>
      <td>92.0</td>
      <td>92.0</td>
      <td>84.0</td>
      <td>59.0</td>
      <td>45.0</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>190871</td>
      <td>Neymar</td>
      <td>Neymar da Silva Santos Jr.</td>
      <td>Paris Saint-Germain</td>
      <td>https://cdn.sofifa.org/18/teams/73.png</td>
      <td>2100</td>
      <td>25</td>
      <td>French Ligue 1</td>
      <td>1992-02-05</td>
      <td>175.0</td>
      <td>68.0</td>
      <td>Neymar</td>
      <td>True</td>
      <td>https://cdn.sofifa.org/flags/54@3x.png</td>
      <td>Brazil</td>
      <td>https://cdn.sofifa.org/18/players/190871.png</td>
      <td>123000000.0</td>
      <td>280000.0</td>
      <td>236800000.0</td>
      <td>92</td>
      <td>94</td>
      <td>92</td>
      <td>84</td>
      <td>79</td>
      <td>95</td>
      <td>30</td>
      <td>60</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>High</td>
      <td>Medium</td>
      <td>Right</td>
      <td>75</td>
      <td>89</td>
      <td>62</td>
      <td>81</td>
      <td>83</td>
      <td>96</td>
      <td>81</td>
      <td>84</td>
      <td>75</td>
      <td>95</td>
      <td>94</td>
      <td>90</td>
      <td>96</td>
      <td>88</td>
      <td>82</td>
      <td>80</td>
      <td>61</td>
      <td>78</td>
      <td>53</td>
      <td>77</td>
      <td>56</td>
      <td>36</td>
      <td>90</td>
      <td>80</td>
      <td>81</td>
      <td>92</td>
      <td>21</td>
      <td>24</td>
      <td>33</td>
      <td>9</td>
      <td>9</td>
      <td>15</td>
      <td>15</td>
      <td>11</td>
      <td>84.0</td>
      <td>89.0</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>79.0</td>
      <td>87.0</td>
      <td>59.0</td>
      <td>46.0</td>
      <td>59.0</td>
      <td>64.0</td>
      <td>84.0</td>
      <td>89.0</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>79.0</td>
      <td>87.0</td>
      <td>59.0</td>
      <td>46.0</td>
      <td>59.0</td>
      <td>64.0</td>
      <td>84.0</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>79.0</td>
      <td>59.0</td>
      <td>46.0</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>176580</td>
      <td>L. Suárez</td>
      <td>Luis Suárez</td>
      <td>FC Barcelona</td>
      <td>https://cdn.sofifa.org/18/teams/241.png</td>
      <td>2291</td>
      <td>30</td>
      <td>Spanish Primera División</td>
      <td>1987-01-24</td>
      <td>182.0</td>
      <td>86.0</td>
      <td>Normal</td>
      <td>True</td>
      <td>https://cdn.sofifa.org/flags/60@3x.png</td>
      <td>Uruguay</td>
      <td>https://cdn.sofifa.org/18/players/176580.png</td>
      <td>97000000.0</td>
      <td>510000.0</td>
      <td>198900000.0</td>
      <td>92</td>
      <td>92</td>
      <td>82</td>
      <td>90</td>
      <td>79</td>
      <td>87</td>
      <td>42</td>
      <td>81</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>High</td>
      <td>Medium</td>
      <td>Right</td>
      <td>77</td>
      <td>94</td>
      <td>77</td>
      <td>83</td>
      <td>88</td>
      <td>86</td>
      <td>86</td>
      <td>84</td>
      <td>64</td>
      <td>91</td>
      <td>88</td>
      <td>77</td>
      <td>86</td>
      <td>93</td>
      <td>60</td>
      <td>87</td>
      <td>69</td>
      <td>89</td>
      <td>80</td>
      <td>86</td>
      <td>78</td>
      <td>41</td>
      <td>92</td>
      <td>84</td>
      <td>85</td>
      <td>83</td>
      <td>30</td>
      <td>45</td>
      <td>38</td>
      <td>27</td>
      <td>25</td>
      <td>31</td>
      <td>33</td>
      <td>37</td>
      <td>88.0</td>
      <td>87.0</td>
      <td>88.0</td>
      <td>87.0</td>
      <td>80.0</td>
      <td>85.0</td>
      <td>65.0</td>
      <td>58.0</td>
      <td>64.0</td>
      <td>68.0</td>
      <td>88.0</td>
      <td>87.0</td>
      <td>88.0</td>
      <td>87.0</td>
      <td>80.0</td>
      <td>85.0</td>
      <td>65.0</td>
      <td>58.0</td>
      <td>64.0</td>
      <td>68.0</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>87.0</td>
      <td>80.0</td>
      <td>65.0</td>
      <td>58.0</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>167495</td>
      <td>M. Neuer</td>
      <td>Manuel Neuer</td>
      <td>FC Bayern Munich</td>
      <td>https://cdn.sofifa.org/18/teams/21.png</td>
      <td>1493</td>
      <td>31</td>
      <td>German Bundesliga</td>
      <td>1986-03-27</td>
      <td>193.0</td>
      <td>92.0</td>
      <td>Normal</td>
      <td>True</td>
      <td>https://cdn.sofifa.org/flags/21@3x.png</td>
      <td>Germany</td>
      <td>https://cdn.sofifa.org/18/players/167495.png</td>
      <td>61000000.0</td>
      <td>230000.0</td>
      <td>100700000.0</td>
      <td>92</td>
      <td>92</td>
      <td>91</td>
      <td>90</td>
      <td>95</td>
      <td>89</td>
      <td>60</td>
      <td>91</td>
      <td>5</td>
      <td>1</td>
      <td>4</td>
      <td>Medium</td>
      <td>Medium</td>
      <td>Right</td>
      <td>15</td>
      <td>13</td>
      <td>25</td>
      <td>55</td>
      <td>11</td>
      <td>30</td>
      <td>14</td>
      <td>11</td>
      <td>59</td>
      <td>48</td>
      <td>58</td>
      <td>61</td>
      <td>52</td>
      <td>85</td>
      <td>35</td>
      <td>25</td>
      <td>78</td>
      <td>44</td>
      <td>83</td>
      <td>16</td>
      <td>29</td>
      <td>30</td>
      <td>12</td>
      <td>70</td>
      <td>47</td>
      <td>70</td>
      <td>10</td>
      <td>10</td>
      <td>11</td>
      <td>91</td>
      <td>90</td>
      <td>95</td>
      <td>91</td>
      <td>89</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>92.0</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
  </tbody>
</table>

</div>


Each row represents a different player, and each player has 185 different stat fields in the dataframe. The columns contain a variety of different data types including strings, ints, and booleans. Let's examine the most common nationalities over the entire dataframe. (We also add a "Rank" column, in case our grouping later on changes the initial indexing.)


```python
fifa['Rank']=fifa.index+1
fifa.groupby('nationality')['ID'].count().sort_values(ascending=False).head(10)
```




    nationality
    England        1631
    Germany        1147
    Spain          1020
    France          966
    Argentina       962
    Brazil          806
    Italy           800
    Colombia        593
    Japan           471
    Netherlands     430
    Name: ID, dtype: int64



Let's also take a look at the 30 least common nationalities.


```python
fifa.groupby('nationality')['ID'].count().sort_values(ascending=False).tail(30)
```




    nationality
    New Caledonia          2
    Kuwait                 2
    Namibia                2
    Cuba                   2
    Ethiopia               2
    Mauritania             2
    Libya                  2
    Gibraltar              2
    Liberia                2
    El Salvador            2
    Oman                   1
    São Tomé & Príncipe    1
    Kyrgyzstan             1
    Hong Kong              1
    Turkmenistan           1
    Barbados               1
    Guatemala              1
    Vietnam                1
    Fiji                   1
    Eritrea                1
    Swaziland              1
    Belize                 1
    Guam                   1
    St Lucia               1
    Sri Lanka              1
    Somalia                1
    San Marino             1
    Burundi                1
    Mauritius              1
    Grenada                1
    Name: ID, dtype: int64



In later explorations, for statistical significance, we will filter out countries that have very few players.

Instead of just looking at the raw number of players from each country, let's see how many players from each country have an overall FIFA rating of 80 or above. (80 is a very high score; these players are ranked as the top 524 players in the world).


```python
fifa[fifa.overall>=80].groupby('nationality')['ID'].count().sort_values(ascending=False).head(10)
```




    nationality
    Spain        80
    Germany      48
    Brazil       48
    France       47
    Italy        37
    England      33
    Argentina    31
    Portugal     21
    Belgium      19
    Croatia      13
    Name: ID, dtype: int64



Comparing this filtered player count to the raw player count, we see that Spain vastly outperforms expectations in terms of great players with 80 players out of 1020 with an overall ranking 80 or higher. On the other hand, England seems to be underperforming with only 33 players out of 1631 meeting the same ranking requirement. Let's compare the distribution of the overall ratings.


```python
sns.kdeplot(fifa[fifa.nationality=='Spain']['overall'], label='Spain', shade=True)
sns.kdeplot(fifa[fifa.nationality=='England']['overall'], label='England', shade=True)
plt.xlabel('Overall Rating')
plt.ylabel('Probability Density')
```




    Text(0,0.5,'Probability Density')




![png](Fifa2018_files/Fifa2018_12_1.png)


This PDF comparison is illuminating, showing how many more elite-level players Spain has as compared to England.

# Positions <a name="positions"></a>
Let's shift our focus towards positions. At the very basic level, soccer positions can be split into Goalkeepers, Defenders, Midfielders, and Forwards. At a more detailed level, there are more nuanced positions and roles such as the False 9, Poacher, No. 10, etc. Let's choose to take a middling approach and categorize the positions into

* GK
* Center-back
* Outside-back
* Center-mid
* Outside-mid
* Forward

The dataframe includes columns indicating which (highly specific) position the player prefers to play. Let's write some code to perform our categorization.


```python
fifa['Position']= np.nan

fifa.loc[fifa.prefers_lm == True, ['Position']] = 'Outside-mid'
fifa.loc[fifa.prefers_rm == True, ['Position']] = 'Outside-mid'
fifa.loc[fifa.prefers_lw == True, ['Position']] = 'Outside-mid'
fifa.loc[fifa.prefers_rw == True, ['Position']] = 'Outside-mid'
fifa.loc[fifa.prefers_cam == True, ['Position']] = 'Center-mid'
fifa.loc[fifa.prefers_cm == True, ['Position']] = 'Center-mid'
fifa.loc[fifa.prefers_cdm == True, ['Position']] = 'Center-mid'
fifa.loc[fifa.prefers_rb == True, ['Position']] = 'Outside-back'
fifa.loc[fifa.prefers_lb == True, ['Position']] = 'Outside-back'
fifa.loc[fifa.prefers_cb == True, ['Position']] = 'Center-back'
fifa.loc[fifa.prefers_st == True, ['Position']] = 'Forward'
fifa.loc[fifa.prefers_cf == True, ['Position']] = 'Forward'
fifa.loc[fifa.prefers_gk == True, ['Position']] = 'GK'

pos_order = ['GK', 'Center-back', 'Outside-back', 'Center-mid', 'Outside-mid', 'Forward']
```

I wonder if any countries excel at producing world-class soccer players for specific positions. Let's take a look at the top 50 players per position, counting the number from each country.


```python
top_gk = fifa[fifa.Position=='GK'].head(50)
top_gk.groupby('nationality')['ID'].count().sort_values(ascending=False).head(5)
```




    nationality
    Spain      8
    Germany    7
    France     5
    Italy      5
    England    3
    Name: ID, dtype: int64




```python
top_cb = fifa[fifa.Position=='Center-back'].head(50)
top_cb.groupby('nationality')['ID'].count().sort_values(ascending=False).head(5)
```




    nationality
    Germany      7
    Spain        5
    Brazil       5
    France       5
    Argentina    4
    Name: ID, dtype: int64




```python
top_def = fifa[fifa.Position=='Outside-back'].head(50)
top_def.groupby('nationality')['ID'].count().sort_values(ascending=False).head(5)
```




    nationality
    Spain       10
    England      7
    Brazil       6
    Portugal     5
    Germany      4
    Name: ID, dtype: int64




```python
top_cm = fifa[fifa.Position=='Center-mid'].head(50)
top_cm.groupby('nationality')['ID'].count().sort_values(ascending=False).head(5)
```




    nationality
    Spain      12
    Germany     6
    Brazil      5
    France      4
    Croatia     3
    Name: ID, dtype: int64




```python
top_om = fifa[fifa.Position=='Outside-mid'].head(50)
top_om.groupby('nationality')['ID'].count().sort_values(ascending=False).head(5)
```




    nationality
    Spain       9
    Brazil      5
    Portugal    4
    France      4
    Italy       3
    Name: ID, dtype: int64




```python
top_for = fifa[fifa.Position=='Forward'].head(50)
top_for.groupby('nationality')['ID'].count().sort_values(ascending=False).head(5)
```




    nationality
    Argentina    6
    Italy        6
    France       6
    Spain        5
    Germany      4
    Name: ID, dtype: int64



We see that Spain tops 4 out of the 6 lists. The most impressive figure here is surely the fact that 12 of the top 50 center midfielders (including attacking and defensive) are Spanish. Let's take a look to see who they are. Soccer fans will recognize these names as some of the best in the world.


```python
key_attr = ['name', 'overall', 'club', 'league', 'nationality']
top_cm[top_cm.nationality=='Spain'][key_attr]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>overall</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>22</th>
      <td>Thiago</td>
      <td>88</td>
      <td>FC Bayern Munich</td>
      <td>German Bundesliga</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>42</th>
      <td>David Silva</td>
      <td>87</td>
      <td>Manchester City</td>
      <td>English Premier League</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Iniesta</td>
      <td>87</td>
      <td>FC Barcelona</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Isco</td>
      <td>86</td>
      <td>Real Madrid CF</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Sergio Busquets</td>
      <td>86</td>
      <td>FC Barcelona</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Cesc Fàbregas</td>
      <td>86</td>
      <td>Chelsea</td>
      <td>English Premier League</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Koke</td>
      <td>85</td>
      <td>Atlético Madrid</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Marco Asensio</td>
      <td>84</td>
      <td>Real Madrid CF</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Ander Herrera</td>
      <td>84</td>
      <td>Manchester United</td>
      <td>English Premier League</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>112</th>
      <td>Bruno</td>
      <td>84</td>
      <td>Villarreal CF</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Juan Mata</td>
      <td>84</td>
      <td>Manchester United</td>
      <td>English Premier League</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Parejo</td>
      <td>83</td>
      <td>Valencia CF</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
    </tr>
  </tbody>
</table>
</div>



# Height and Weight <a name="height"></a>
At the professional level, soccer players must be in peak physical shape to be competitive. However, players come in all shapes and sizes. Let's take a look at a KDE plot with height on the x_axis and weight on the y_axis. Also, the marginal distributions are included outside the plot.


```python
sns.jointplot(x='height_cm', y='weight_kg', data=fifa, kind="kde")
```




    <seaborn.axisgrid.JointGrid at 0x29e62c23588>




![png](Fifa2018_files/Fifa2018_25_1.png)


Let's take a quick glance at the statistics for the height and weight metrics.


```python
fifa.height_cm.describe()
```




    count    17994.000000
    mean       181.271980
    std          6.690392
    min        155.000000
    25%        177.000000
    50%        181.000000
    75%        186.000000
    max        205.000000
    Name: height_cm, dtype: float64




```python
fifa.weight_kg.describe()
```




    count    17994.000000
    mean        75.400856
    std          6.994824
    min         49.000000
    25%         70.000000
    50%         75.000000
    75%         80.000000
    max        110.000000
    Name: weight_kg, dtype: float64



Converting to imperial units, we see that the average height is 5'11.4" (min: 5'1", max: 6'8.7"), and the average weight is 166.2 lbs (min: 108 lbs, max: 242.5 lbs).

Let's see if there are any countries that produce significantly taller or shorter players than other countries. We first filter out any countries that contain fewer than 50 players in the dataframe. Then, we find the countries with the tallest and shortest average heights for their players.



```python
fifa_sig = fifa.groupby('nationality')
fifa_sig = fifa_sig.filter(lambda x:x['nationality'].count()>=50)
fifa_sig.groupby('nationality')['height_cm'].mean().sort_values(ascending=False).head(5)
```




    nationality
    Bosnia Herzegovina    185.625000
    Serbia                184.736842
    Czech Republic        184.698630
    Croatia               184.373832
    Senegal               183.952381
    Name: height_cm, dtype: float64




```python
fifa_sig.groupby('nationality')['height_cm'].mean().sort_values(ascending=False).head(5)
fifa_sig.groupby('nationality')['height_cm'].mean().sort_values().head(5)
```




    nationality
    Saudi Arabia    175.654434
    South Africa    176.387500
    Mexico          176.980609
    Japan           177.794055
    Ghana           178.280702
    Name: height_cm, dtype: float64



Let's create a KDE plot comparing the heights and weights of the two countries at the extremes.


```python
bosnia=fifa[fifa.nationality=='Bosnia Herzegovina']
saudi=fifa[fifa.nationality=='Saudi Arabia']
sns.kdeplot(bosnia.height_cm, bosnia.weight_kg, cmap='Reds')
sns.kdeplot(saudi.height_cm, saudi.weight_kg, cmap='Blues')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29e635b2390>




![png](Fifa2018_files/Fifa2018_33_1.png)


The plots clearly shows that players from Bosnia Herzegovina tend to be significantly taller and heavier than the players from Saudi Arabia. This agrees with various lists of [average human height worldwide](https://en.wikipedia.org/wiki/List_of_average_human_height_worldwide), all of which list Bosnia Herzegovina either first or second in terms of average height (and Saudi Arabia near the bottom).

Let's shift our attention to the body composition per position. We use a boxplot; the box itself contains the interquartile range (IQR), i.e., from 25% to 75%. The whiskers extend above and below the box, each with a length of $1.5*IQR$. Outside of the whiskers, the outliers are included as marks. The notch in the box displays the median of the data.


```python
sns.boxplot(x='Position', y='height_cm', data=fifa, order=pos_order)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29e639911d0>




![png](Fifa2018_files/Fifa2018_35_1.png)


Wow, there are a few notable things to point out.

* The goalkeepers are tallest, which is to be expected since there is a high correlation between height and wingspan.
* The IQR of the center-backs and the outside-backs do not even overlap! This makes sense and highlights why we separated the two defender positions. Center-backs tend to be tall so that they can win headers in and around the 18-yard box. Outside-backs need to man-mark speedy forwards, and thus tend to be smaller in stature.
* The outside-mids are the shortest on average. They need to be able to fly down the wings at incredible speeds to deliver crosses to the trailing forwards.

It appears that forwards have the most spread out height distribution. Let's glance at the violin plot for further details.


```python
sns.violinplot(x='Position', y='height_cm', data=fifa, order=pos_order)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29e644576d8>




![png](Fifa2018_files/Fifa2018_37_1.png)


Yes, it is clear that the forwards are the most spread out in terms of height. For numerical data, we can turn to the standard deviation.


```python
fifa.groupby('Position')['height_cm'].std()
```




    Position
    Center-back     4.766720
    Center-mid      5.793662
    Forward         6.515699
    GK              4.604998
    Outside-back    5.088083
    Outside-mid     5.319471
    Name: height_cm, dtype: float64



Let's calculate the linear correlation between height and weight per position.


```python
fifa.groupby('Position')[['height_cm', 'weight_kg']].corr()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>height_cm</th>
      <th>weight_kg</th>
    </tr>
    <tr>
      <th>Position</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">Center-back</th>
      <th>height_cm</th>
      <td>1.000000</td>
      <td>0.625164</td>
    </tr>
    <tr>
      <th>weight_kg</th>
      <td>0.625164</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Center-mid</th>
      <th>height_cm</th>
      <td>1.000000</td>
      <td>0.700717</td>
    </tr>
    <tr>
      <th>weight_kg</th>
      <td>0.700717</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Forward</th>
      <th>height_cm</th>
      <td>1.000000</td>
      <td>0.731472</td>
    </tr>
    <tr>
      <th>weight_kg</th>
      <td>0.731472</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">GK</th>
      <th>height_cm</th>
      <td>1.000000</td>
      <td>0.587178</td>
    </tr>
    <tr>
      <th>weight_kg</th>
      <td>0.587178</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Outside-back</th>
      <th>height_cm</th>
      <td>1.000000</td>
      <td>0.633062</td>
    </tr>
    <tr>
      <th>weight_kg</th>
      <td>0.633062</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Outside-mid</th>
      <th>height_cm</th>
      <td>1.000000</td>
      <td>0.665286</td>
    </tr>
    <tr>
      <th>weight_kg</th>
      <td>0.665286</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



The position with the lowest correlation between height and weight is goalkeeper. Since goalkeepers aren't required to run miles on end during the game, they can come in a variety of shapes and sizes. While some goalkeepers are extremely tall and lanky, others are short and stout (with unusually long arms).

# Top 5 Leagues <a name="leagues"></a>

It is widely acknowledged that the top 5 leagues in the world are the
* Spanish Primera División
* English Premier League
* Italian Serie A
* German Bundesliga
* French Ligue 1.

While the exact order is up for debate, I will keep them in this order, as per the [current UEFA coefficients](https://www.uefa.com/memberassociations/uefarankings/country/).
 To begin with, let's take a look at the average overall score per player per league.


```python
leagues=['Spanish Primera División', 'English Premier League', 'Italian Serie A', 'German Bundesliga', 'French Ligue 1']
fifa_sub = fifa.loc[fifa.league.isin(leagues)]
fifa_sub.groupby('league')['overall'].mean()
```




    league
    English Premier League      72.409786
    French Ligue 1              70.453177
    German Bundesliga           72.428305
    Italian Serie A             72.588551
    Spanish Primera División    73.686047
    Name: overall, dtype: float64



It seems that the Spanish league has the highest average quality of players and that the French league is far behind the other 4. Let's look at a strip plot, conditioned on position, to get a better sense of things.


```python
_ = sns.stripplot(x='league', y='overall', hue='Position', data=fifa_sub, dodge=0.6, jitter=True, alpha=.5, order=leagues, hue_order=pos_order)
loc, labels = plt.xticks()
_.set_xticklabels(labels, rotation=45)
plt.legend(bbox_to_anchor=(1.04,1), loc="upper left")
```




    <matplotlib.legend.Legend at 0x29e646b2b70>




![png](Fifa2018_files/Fifa2018_46_1.png)


It's tough to glean insight from this plot due to the sheer number of points. A boxplot will be more useful.


```python
_=sns.boxplot(x='league', y='overall', hue='Position', data=fifa_sub, order=leagues, hue_order=pos_order)
loc, labels = plt.xticks()
_.set_xticklabels(labels, rotation=45)
plt.legend(bbox_to_anchor=(1.04,1), loc="upper left")
```




    <matplotlib.legend.Legend at 0x29e646b65c0>




![png](Fifa2018_files/Fifa2018_48_1.png)


It is clear that the Spanish teams have a higher lower-floor, i.e., their teams have much greater depth from the bench. While the English league seems to be well balanced by position, note that the outside-mids in the Italian league are far worse than the other positions, while the outside-mids in the German league are far better. To savvy soccer fans, this last observation makes perfect sense; the Bundesliga is known for its frenetic pace, sending wingers tearing down the sidelines, while the Serie A has a slower, more defense, play-not-to-lose style.

Let's take a look at the rankings of the top 50 players in each league. (Since this is a ranking, the lower the number the better).


```python
fifa_sub_top = fifa_sub.groupby('league').head(50)
_ = sns.boxplot(x='league', y='Rank', data=fifa_sub_top, order=leagues)
_ = sns.stripplot(x='league', y='Rank', data=fifa_sub_top, order=leagues)
loc, labels = plt.xticks()
_.set_xticklabels(labels, rotation=45)
```




    [Text(0,0,'Spanish Primera División'),
     Text(0,0,'English Premier League'),
     Text(0,0,'Italian Serie A'),
     Text(0,0,'German Bundesliga'),
     Text(0,0,'French Ligue 1')]




![png](Fifa2018_files/Fifa2018_50_1.png)


It's very interesting to see that when just looking at the top 50 players in each league, the English league appears have the best ranking. However, with rankings, there are some nuances with boxplots that don't make them the best visual representation. Let's instead turn to a stackplot. The x-axis will represent the cumulative rankings, and the y-axis shows the percentage of players ranked at that point (i.e., at x=10, players ranked 1-10 will be represented) per country. Let's first perform this visual up to rank 100, then we will expand to 1000.


```python
top=100
fifa_top = fifa.head(top)

top_vec=[None]*(len(leagues))
for L in range(len(leagues)):
    top_vec[L] = [((fifa_top.Rank<=x+1) & (fifa_top.league==leagues[L])).sum() for x in range(top)]
#need to calculate the 'other' league
other = [None]*top
top_vec_numpy = np.array(top_vec)
for r in range(top):
    other[r] = r-np.sum(top_vec_numpy[:,r])+1

#now let's make the data frame
top_df = pd.DataFrame({leagues[0]:top_vec[0],
                     leagues[1]:top_vec[1],
                     leagues[2]:top_vec[2],
                     leagues[3]:top_vec[3],
                     leagues[4]:top_vec[4],
                     'Other Leagues':other}, index=range(1,top+1))

top_df_perc = top_df.divide(top_df.sum(axis=1), axis=0)
plt.stackplot(range(1,top+1), top_df_perc[leagues[0]], top_df_perc[leagues[1]],
              top_df_perc[leagues[2]], top_df_perc[leagues[3]],
              top_df_perc[leagues[4]], top_df_perc['Other Leagues'],
              labels=[*leagues, 'Other Leagues'])
plt.legend(bbox_to_anchor=(1.04,1), loc="upper left")
plt.margins(0,0)
plt.title('Player Ranking Stacked Chart per League')
plt.xlabel('Player Ranking')
```




    Text(0.5,0,'Player Ranking')




![png](Fifa2018_files/Fifa2018_52_1.png)


It appears that the Spanish league has a high-percentage of the very, very best players in the world. The French league gets an early boost from Neymar, ranked 3rd in the world, but doesn't get another player ranked until rank 30. Let's take a look at the percentages at the top 10 and the top 100.


```python
#top-10
top_df_perc.iloc[9]
```




    English Premier League      0.2
    French Ligue 1              0.1
    German Bundesliga           0.2
    Italian Serie A             0.1
    Other Leagues               0.0
    Spanish Primera División    0.4
    Name: 10, dtype: float64




```python
#top-100
top_df_perc.iloc[-1]
```




    English Premier League      0.32
    French Ligue 1              0.08
    German Bundesliga           0.16
    Italian Serie A             0.16
    Other Leagues               0.01
    Spanish Primera División    0.27
    Name: 100, dtype: float64



While the English league gets off to a slow start, they actually have more top-100 players than any other league. One thing that is remarkable is the utter dominance of the top 5 leagues. There is only 1 player ranked in the top 100 who is not in the main 5 leagues. Let's find out who it is.


```python
fifa.loc[~fifa.league.isin(leagues)][['name', 'Rank', 'club', 'league', 'nationality', 'overall']].head(1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Rank</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
      <th>overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>67</th>
      <td>Pepe</td>
      <td>68</td>
      <td>Beşiktaş JK</td>
      <td>Turkish Süper Lig</td>
      <td>Portugal</td>
      <td>86</td>
    </tr>
  </tbody>
</table>
</div>



We now perform the same actions, but this time we get the stack plot for the top 1000 players instead of the top 100.


```python
top=1000
fifa_top = fifa.head(top)

top_vec=[None]*(len(leagues))
for L in range(len(leagues)):
    top_vec[L] = [((fifa_top.Rank<=x+1) & (fifa_top.league==leagues[L])).sum() for x in range(top)]
#need to calculate the 'other' league
other = [None]*top
top_vec_numpy = np.array(top_vec)
for r in range(top):
    other[r] = r-np.sum(top_vec_numpy[:,r])+1

#now let's make the data frame
top_df = pd.DataFrame({leagues[0]:top_vec[0],
                     leagues[1]:top_vec[1],
                     leagues[2]:top_vec[2],
                     leagues[3]:top_vec[3],
                     leagues[4]:top_vec[4],
                     'Other Leagues':other}, index=range(1,top+1))

top_df_perc = top_df.divide(top_df.sum(axis=1), axis=0)
plt.stackplot(range(1,top+1), top_df_perc[leagues[0]], top_df_perc[leagues[1]],
              top_df_perc[leagues[2]], top_df_perc[leagues[3]],
              top_df_perc[leagues[4]], top_df_perc['Other Leagues'],
              labels=[*leagues, 'Other Leagues'])
plt.legend(bbox_to_anchor=(1.04,1), loc="upper left")
plt.margins(0,0)
plt.title('Player Ranking Stacked Chart per League')
plt.xlabel('Player Ranking')
```




    Text(0.5,0,'Player Ranking')




![png](Fifa2018_files/Fifa2018_59_1.png)



```python
#top-1000
top_df_perc.iloc[-1]
```




    English Premier League      0.205
    French Ligue 1              0.077
    German Bundesliga           0.139
    Italian Serie A             0.137
    Other Leagues               0.241
    Spanish Primera División    0.201
    Name: 1000, dtype: float64



These figures clearly demonstrate that the English and Spanish leagues are ahead of their competitors. Almost every starting player in these two leagues are ranked in the top 1000.

# England vs. Germany PKs <a name="PKs"></a>

When it comes to Penalty Kicks in international competitions, two teams clearly stand out from the norm: England and Germany. England has a woeful history with PKs, losing all 3 in World Cup competitions, and winning only 1 out of 7 including the European Cup. Their relentless choking during PKs is well documented ([1](https://www.theguardian.com/football/2016/jun/27/england-penalties-20-years-hurt-iceland-germany), [2](https://www.telegraph.co.uk/men/the-filter/10843004/Why-are-England-so-bad-at-penalty-shoot-outs.html), [3](http://blogs.bcu.ac.uk/views/2016/06/15/why-are-england-so-bad-at-penalty-shootouts/), [4](https://www.ft.com/content/f32c2ac6-ec4a-11e3-ab1b-00144feabdc0)) and may be adding to the continuing pressure in a [feedback loop](http://www.popularsocialscience.com/2012/11/28/the-psychology-of-penalty-shootouts-why-do-england-always-lose/). At the opposite end of the spectrum, the German team is legendary for their calm demeanor in expertly executing PKs, winning [all 4](https://en.wikipedia.org/wiki/List_of_FIFA_World_Cup_penalty_shoot-outs) of their PKs during World Cups. In fact, their last PK loss (in the WC or Euro) dates back more than 42 years, at the Euro 1976. There are very few things in international soccer as sure bets as England losing a PK and Germany winning.

FIFA 18 includes a **Penalty** stat that describes how good a player is at taking a penalty kick. Let's investigate the differences between England and Germany.


```python
countries = ['England', 'Germany']
fifa_pen = fifa.loc[fifa.nationality.isin(countries)]
sns.violinplot(x='nationality', y='penalties',  data=fifa_pen, inner='quartile')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29e6496a438>




![png](Fifa2018_files/Fifa2018_64_1.png)


This plot is surprising, it seems that English players have the slight edge over German player in the penalties stat. Let's break this down by position; it could be that players who are unlikely to take PKs in the first place are bringing down the German distribution.


```python
sns.violinplot(x='Position', y='penalties', hue='nationality', data=fifa_pen, split=True, inner='quartile', order=pos_order)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29e64b3a5f8>




![png](Fifa2018_files/Fifa2018_66_1.png)


Wow, it seems like England really does have the edge over Germany in penalty stats for offensive players.

PKs are a two-sided coin though, and goalkeepers play a huge role in the outcome. Let's see if there are any differences in the overall quality of GKs between these two countries.


```python
sns.violinplot(x='nationality', y='overall', data=fifa_pen[fifa_pen.Position=='GK'], inner='quartile')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29e64b58cf8>




![png](Fifa2018_files/Fifa2018_68_1.png)


This is quite illuminating. The median overall metric for German GKs is at the same level as the 75th percentile overall metric for English GKs. Also note that England doesn't seem to really have any world-class GKs at the top of the violin plot. Since only the very best GKs per country really matter during an international tournament, let's see how the top GKs from each country compare.


```python
fifa_pen.loc[(fifa_pen['Position']=='GK') & (fifa_pen['nationality']=='Germany')][['name', 'club', 'overall']].head(7)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>M. Neuer</td>
      <td>FC Bayern Munich</td>
      <td>92</td>
    </tr>
    <tr>
      <th>79</th>
      <td>B. Leno</td>
      <td>Bayer 04 Leverkusen</td>
      <td>85</td>
    </tr>
    <tr>
      <th>80</th>
      <td>M. ter Stegen</td>
      <td>FC Barcelona</td>
      <td>85</td>
    </tr>
    <tr>
      <th>104</th>
      <td>T. Horn</td>
      <td>1. FC Köln</td>
      <td>84</td>
    </tr>
    <tr>
      <th>117</th>
      <td>R. Fährmann</td>
      <td>FC Schalke 04</td>
      <td>84</td>
    </tr>
    <tr>
      <th>240</th>
      <td>O. Baumann</td>
      <td>TSG 1899 Hoffenheim</td>
      <td>82</td>
    </tr>
    <tr>
      <th>353</th>
      <td>K. Trapp</td>
      <td>Paris Saint-Germain</td>
      <td>81</td>
    </tr>
  </tbody>
</table>
</div>




```python
fifa_pen.loc[(fifa_pen['Position']=='GK') & (fifa_pen['nationality']=='England')][['name', 'club', 'overall']].head(7)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>288</th>
      <td>J. Hart</td>
      <td>West Ham United</td>
      <td>82</td>
    </tr>
    <tr>
      <th>320</th>
      <td>J. Butland</td>
      <td>Stoke City</td>
      <td>81</td>
    </tr>
    <tr>
      <th>376</th>
      <td>T. Heaton</td>
      <td>Burnley</td>
      <td>81</td>
    </tr>
    <tr>
      <th>502</th>
      <td>B. Foster</td>
      <td>West Bromwich Albion</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1057</th>
      <td>J. Pickford</td>
      <td>Everton</td>
      <td>77</td>
    </tr>
    <tr>
      <th>1109</th>
      <td>F. Forster</td>
      <td>Southampton</td>
      <td>77</td>
    </tr>
    <tr>
      <th>1812</th>
      <td>A. McCarthy</td>
      <td>Southampton</td>
      <td>75</td>
    </tr>
  </tbody>
</table>
</div>



These latest stats provide some deep insight. The 6 best German GKs are all ranked higher than J. Hart, the highest rated English GK. In addition to the mounting pressure the English players feel to break the PK curse, it can't help that they do not have a single choice for a world-class GK. On the other hand, the German players can keep their nerve, knowing they have a choice between some of the best GKs in the world.

# Southpaws <a name="southpaws"></a>
Lefties make up roughly [10%](https://en.wikipedia.org/wiki/Handedness) of the population. However, it is well documented that in some sports, lefties make up a much larger proportion (e.g., 39% of hitters and 28% of pitchers [played left-handed](http://www.gamesensesports.com/knowledge/2017/3/17/righties-vs-lefties-the-importance-of-handedness-training-in-baseball-hitting) in the 2012 MLB season).

In soccer, we expect to see higher than 10% of players as left-footed since being left-footed is an advantage when playing on the left side of the field. This is especially true for left wingers who race down the sideline in order to whip in a cross. Let's investigate the percentages.


```python
fifa[fifa.preferred_foot=='Left']['ID'].count()/fifa.shape[0]
```




    0.23624541513837946



We see that 23% of the players in the dataframe are left-footed, which agrees with our intuition. Let's see if there is any significant difference in the overall quality of players based on foot preference.


```python
fifa.groupby('preferred_foot')['overall'].mean()
```




    preferred_foot
    Left     66.679840
    Right    66.121007
    Name: overall, dtype: float64



These scores are very close together, hinting that there is no discernable difference between the overall quality of left-footed player and right-footed players on average. However, in order to see if this difference of 0.5 is likely due to noise, our analysis must go one level deeper. Here, we use the [bootstrapping method](https://en.wikipedia.org/wiki/Bootstrapping_(statistics)), in which we resample our players (with replacement). The advantage of the bootstrapping method is that it gives us intuition as to whether this 0.5 difference is statistically significant without having to assume any underlying distribution.


```python
iterations=10000
boot=[]
for i in range(iterations):
    boot_mean = fifa.sample(frac=1, replace=True).groupby('preferred_foot')['overall'].mean()
    boot.append(boot_mean)

boot=pd.DataFrame(boot)
boot.plot.kde()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29e664d26a0>




![png](Fifa2018_files/Fifa2018_78_1.png)


It does appear that the difference, albeit small, is real. In fact, over the 10000 bootstrapping iterations, not once did the right-footers have a higher overall mean than the left-footers (as shown with the following code).


```python
((boot['Left']-boot['Right'])<0).sum()
```




    0



Let's breakdown the left-footed percentages by nationality. We only consider countries that have 100 or more players in the dataframe.


```python
fifa_sig = fifa.groupby('nationality')
fifa_sig = fifa_sig.filter(lambda x:x['nationality'].count()>=100)

fifa_left = pd.DataFrame(fifa_sig[fifa_sig.preferred_foot=='Left'].groupby('nationality')['ID'].count())
fifa_left['left_perc']= fifa_left['ID'].divide(fifa_sig.groupby('nationality')['ID'].count())
fifa_left_sorted = fifa_left.sort_values('left_perc')

_ = fifa_left_sorted.left_perc.plot.bar(rot=90)
plt.ylabel('Percentage of Players Left-Footed')
```




    Text(0,0.5,'Percentage of Players Left-Footed')




![png](Fifa2018_files/Fifa2018_82_1.png)


Glancing at the order of countries, there doesn't appear to be any geographic or political correlations with the percentage of left-footed players.

Let's try to go one step deeper with a bubble plot.
* On the x-axis, we take the average overall score for the best 11 players per country.
* On the y-axis, we have the percentage of total players in the country that are left-footed.
* The size of the bubble corresponds to the raw number of player (for the given country) that are ranked in the top 1500.


```python
fifa_best11 = fifa_sig.groupby('nationality').head(11)
#calculate average overall raiting per coutnries best 11
fifa_best11 = fifa_best11.groupby('nationality')['overall'].mean()
fifa_left['Overall'] = fifa_best11
#now we want to calculate the number per country in the top 1000
fifa_top1000 = fifa_sig[fifa_sig.Rank<=1500].groupby('nationality')['ID'].count()

#bubble plot
plt.scatter(fifa_best11, fifa_left.left_perc, s=10*fifa_top1000, alpha=0.5)
plt.ylabel('Percentage of Players Left-Footed')
plt.xlabel('Average Overall Rating if Top 11 Players')
```




    Text(0.5,0,'Average Overall Rating if Top 11 Players')




![png](Fifa2018_files/Fifa2018_84_1.png)


It is impossible to draw correlation/causation results from this plot. However, it is worthwhile to note that the 7 countries at the elite level (who's average overall score for their starting-11 is over 85) also have a higher than average percentage of left-footed players.

# Similarity <a name="similarity"></a>

For fun, let's build a function such that when given a name, it returns the 5 players who are the most similar. The idea of similarity when it comes to soccer players is subjective, but we will be basing it solely on the subjective numeric stats. We will not include any categorical features such as club, league, nationality, position, etc. We also will not include objective physical features such as age, height and weight. There are two major steps to this process.
* First, we perform principle component analysis (PCA). We will be using 45 different metrics to begin with, but many of these are highly correlated with one another. We will use PCA to reduce this number to 10. This reduction in dimension helps to reduce the bias possibly introduced by EA's selection of specific metrics that are similar. Note that these remaining 10 dimensions do not necessarily correspond to any specific metrics, but rather a correlated cluster of them.
* Second, we perform the nearest neighbors calculation to find the closest neighbors based on the numbers in reduced dimensions.


```python
fifa_stats1 = fifa.loc[:,'overall':'weak_foot']
fifa_stats2 = fifa.loc[:,'crossing':'gk_reflexes']
fifa_stats = pd.concat([fifa_stats1, fifa_stats2], axis=1)
with pd.option_context('display.max_rows', None, 'display.max_columns', None):
    display(fifa_stats.head())
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>overall</th>
      <th>potential</th>
      <th>pac</th>
      <th>sho</th>
      <th>pas</th>
      <th>dri</th>
      <th>def</th>
      <th>phy</th>
      <th>international_reputation</th>
      <th>skill_moves</th>
      <th>weak_foot</th>
      <th>crossing</th>
      <th>finishing</th>
      <th>heading_accuracy</th>
      <th>short_passing</th>
      <th>volleys</th>
      <th>dribbling</th>
      <th>curve</th>
      <th>free_kick_accuracy</th>
      <th>long_passing</th>
      <th>ball_control</th>
      <th>acceleration</th>
      <th>sprint_speed</th>
      <th>agility</th>
      <th>reactions</th>
      <th>balance</th>
      <th>shot_power</th>
      <th>jumping</th>
      <th>stamina</th>
      <th>strength</th>
      <th>long_shots</th>
      <th>aggression</th>
      <th>interceptions</th>
      <th>positioning</th>
      <th>vision</th>
      <th>penalties</th>
      <th>composure</th>
      <th>marking</th>
      <th>standing_tackle</th>
      <th>sliding_tackle</th>
      <th>gk_diving</th>
      <th>gk_handling</th>
      <th>gk_kicking</th>
      <th>gk_positioning</th>
      <th>gk_reflexes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>94</td>
      <td>94</td>
      <td>90</td>
      <td>93</td>
      <td>82</td>
      <td>90</td>
      <td>33</td>
      <td>80</td>
      <td>5</td>
      <td>5</td>
      <td>4</td>
      <td>85</td>
      <td>94</td>
      <td>88</td>
      <td>83</td>
      <td>88</td>
      <td>91</td>
      <td>81</td>
      <td>76</td>
      <td>77</td>
      <td>93</td>
      <td>89</td>
      <td>91</td>
      <td>89</td>
      <td>96</td>
      <td>63</td>
      <td>94</td>
      <td>95</td>
      <td>92</td>
      <td>80</td>
      <td>92</td>
      <td>63</td>
      <td>29</td>
      <td>95</td>
      <td>85</td>
      <td>85</td>
      <td>95</td>
      <td>22</td>
      <td>31</td>
      <td>23</td>
      <td>7</td>
      <td>11</td>
      <td>15</td>
      <td>14</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>93</td>
      <td>93</td>
      <td>89</td>
      <td>90</td>
      <td>86</td>
      <td>96</td>
      <td>26</td>
      <td>61</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>77</td>
      <td>95</td>
      <td>71</td>
      <td>88</td>
      <td>85</td>
      <td>97</td>
      <td>89</td>
      <td>90</td>
      <td>87</td>
      <td>95</td>
      <td>92</td>
      <td>87</td>
      <td>90</td>
      <td>95</td>
      <td>95</td>
      <td>85</td>
      <td>68</td>
      <td>73</td>
      <td>59</td>
      <td>88</td>
      <td>48</td>
      <td>22</td>
      <td>93</td>
      <td>90</td>
      <td>78</td>
      <td>96</td>
      <td>13</td>
      <td>28</td>
      <td>26</td>
      <td>6</td>
      <td>11</td>
      <td>15</td>
      <td>14</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>92</td>
      <td>94</td>
      <td>92</td>
      <td>84</td>
      <td>79</td>
      <td>95</td>
      <td>30</td>
      <td>60</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>75</td>
      <td>89</td>
      <td>62</td>
      <td>81</td>
      <td>83</td>
      <td>96</td>
      <td>81</td>
      <td>84</td>
      <td>75</td>
      <td>95</td>
      <td>94</td>
      <td>90</td>
      <td>96</td>
      <td>88</td>
      <td>82</td>
      <td>80</td>
      <td>61</td>
      <td>78</td>
      <td>53</td>
      <td>77</td>
      <td>56</td>
      <td>36</td>
      <td>90</td>
      <td>80</td>
      <td>81</td>
      <td>92</td>
      <td>21</td>
      <td>24</td>
      <td>33</td>
      <td>9</td>
      <td>9</td>
      <td>15</td>
      <td>15</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>92</td>
      <td>92</td>
      <td>82</td>
      <td>90</td>
      <td>79</td>
      <td>87</td>
      <td>42</td>
      <td>81</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>77</td>
      <td>94</td>
      <td>77</td>
      <td>83</td>
      <td>88</td>
      <td>86</td>
      <td>86</td>
      <td>84</td>
      <td>64</td>
      <td>91</td>
      <td>88</td>
      <td>77</td>
      <td>86</td>
      <td>93</td>
      <td>60</td>
      <td>87</td>
      <td>69</td>
      <td>89</td>
      <td>80</td>
      <td>86</td>
      <td>78</td>
      <td>41</td>
      <td>92</td>
      <td>84</td>
      <td>85</td>
      <td>83</td>
      <td>30</td>
      <td>45</td>
      <td>38</td>
      <td>27</td>
      <td>25</td>
      <td>31</td>
      <td>33</td>
      <td>37</td>
    </tr>
    <tr>
      <th>4</th>
      <td>92</td>
      <td>92</td>
      <td>91</td>
      <td>90</td>
      <td>95</td>
      <td>89</td>
      <td>60</td>
      <td>91</td>
      <td>5</td>
      <td>1</td>
      <td>4</td>
      <td>15</td>
      <td>13</td>
      <td>25</td>
      <td>55</td>
      <td>11</td>
      <td>30</td>
      <td>14</td>
      <td>11</td>
      <td>59</td>
      <td>48</td>
      <td>58</td>
      <td>61</td>
      <td>52</td>
      <td>85</td>
      <td>35</td>
      <td>25</td>
      <td>78</td>
      <td>44</td>
      <td>83</td>
      <td>16</td>
      <td>29</td>
      <td>30</td>
      <td>12</td>
      <td>70</td>
      <td>47</td>
      <td>70</td>
      <td>10</td>
      <td>10</td>
      <td>11</td>
      <td>91</td>
      <td>90</td>
      <td>95</td>
      <td>91</td>
      <td>89</td>
    </tr>
  </tbody>
</table>
</div>


First we standardize all the metrics to have a mean of zero and unit variance. This standardization provides a fair way to measure the various attributes since they have different means and distributions.


```python
from sklearn.preprocessing import StandardScaler
fifa_stats = StandardScaler().fit_transform(fifa_stats)
```


```python
#PCA
from sklearn.decomposition import PCA
pca = PCA(n_components=10)
components = pca.fit_transform(fifa_stats)
```


```python
#NearestNeighbors
from sklearn.neighbors import NearestNeighbors
nbrs = NearestNeighbors(n_neighbors=6).fit(components)
distances, indices = nbrs.kneighbors(components)
```

Now that we have the nearest neighbors indices, we are ready to create our function. (Both the *fifa* dataframe and the *indices* numpy array are accessible in the scope of the following function.)


```python
def similar_players(player):
    #First let's make sure there is exactly one player with the matching name
    if fifa[fifa.name==player].size==0:
        print('No player by that name.')
        return
    elif fifa[fifa.name==player].shape[0]>1:
        print('Multiple players by that name, sorry.')
        return
    #First we get the index of the player
    idx = fifa[fifa.name==player].index[0]
    #Get the indices of the 5 neighbors
    n_idx = indices[idx,1:]
    #Subset the fifa dataframe
    fifa_similar = fifa.iloc[n_idx]
    #Print out the results
    display(fifa_similar.loc[:,['name', 'club', 'league', 'nationality', 'Position']])

```

Now it's time to try it out. Even though we didn't pass in information about the players’ position, it stands to reason that the most similar players would have similar roles. Let's try the function with a few players.


```python
#My favorite defender, John Brooks
similar_players('J. Brooks')
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
      <th>Position</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>551</th>
      <td>A. Romagnoli</td>
      <td>Milan</td>
      <td>Italian Serie A</td>
      <td>Italy</td>
      <td>Center-back</td>
    </tr>
    <tr>
      <th>370</th>
      <td>D. Astori</td>
      <td>Fiorentina</td>
      <td>Italian Serie A</td>
      <td>Italy</td>
      <td>Center-back</td>
    </tr>
    <tr>
      <th>303</th>
      <td>D. Sánchez</td>
      <td>Tottenham Hotspur</td>
      <td>English Premier League</td>
      <td>Colombia</td>
      <td>Center-back</td>
    </tr>
    <tr>
      <th>417</th>
      <td>M. Nastasić</td>
      <td>FC Schalke 04</td>
      <td>German Bundesliga</td>
      <td>Serbia</td>
      <td>Center-back</td>
    </tr>
    <tr>
      <th>219</th>
      <td>A. Rüdiger</td>
      <td>Chelsea</td>
      <td>English Premier League</td>
      <td>Germany</td>
      <td>Center-back</td>
    </tr>
  </tbody>
</table>
</div>



```python
#Best American prospect, Christian Pulisic
similar_players('C. Pulisic')
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
      <th>Position</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1484</th>
      <td>Daniel Podence</td>
      <td>Sporting CP</td>
      <td>Portuguese Primeira Liga</td>
      <td>Portugal</td>
      <td>Forward</td>
    </tr>
    <tr>
      <th>1207</th>
      <td>L. Bailey</td>
      <td>Bayer 04 Leverkusen</td>
      <td>German Bundesliga</td>
      <td>Jamaica</td>
      <td>Outside-mid</td>
    </tr>
    <tr>
      <th>835</th>
      <td>L. Acosta</td>
      <td>Club Atlético Lanús</td>
      <td>Argentinian Superliga</td>
      <td>Argentina</td>
      <td>Outside-mid</td>
    </tr>
    <tr>
      <th>1009</th>
      <td>J. Aquino</td>
      <td>Tigres U.A.N.L.</td>
      <td>Mexican Liga MX</td>
      <td>Mexico</td>
      <td>Outside-mid</td>
    </tr>
    <tr>
      <th>3303</th>
      <td>B. Fernández</td>
      <td>FC Metz</td>
      <td>French Ligue 1</td>
      <td>Argentina</td>
      <td>Forward</td>
    </tr>
  </tbody>
</table>
</div>



```python
#Best player on the LA Galaxy (imo)
similar_players('R. Alessandrini')
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
      <th>Position</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>584</th>
      <td>Y. Konoplyanka</td>
      <td>FC Schalke 04</td>
      <td>German Bundesliga</td>
      <td>Ukraine</td>
      <td>Outside-mid</td>
    </tr>
    <tr>
      <th>560</th>
      <td>E. Višća</td>
      <td>İstanbul Başakşehir FK</td>
      <td>Turkish Süper Lig</td>
      <td>Bosnia Herzegovina</td>
      <td>Outside-mid</td>
    </tr>
    <tr>
      <th>965</th>
      <td>Y. Salibur</td>
      <td>En Avant de Guingamp</td>
      <td>French Ligue 1</td>
      <td>France</td>
      <td>Outside-mid</td>
    </tr>
    <tr>
      <th>607</th>
      <td>A. Ljajić</td>
      <td>Torino</td>
      <td>Italian Serie A</td>
      <td>Serbia</td>
      <td>Center-mid</td>
    </tr>
    <tr>
      <th>1989</th>
      <td>M. Oršić</td>
      <td>Ulsan Hyundai Horang-i</td>
      <td>Korean K League Classic</td>
      <td>Croatia</td>
      <td>Outside-mid</td>
    </tr>
  </tbody>
</table>
</div>



```python
#Best player in MLS
similar_players('S. Giovinco')
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
      <th>Position</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>301</th>
      <td>Malcom</td>
      <td>Girondins de Bordeaux</td>
      <td>French Ligue 1</td>
      <td>Brazil</td>
      <td>Outside-mid</td>
    </tr>
    <tr>
      <th>354</th>
      <td>R. Boudebouz</td>
      <td>Real Betis Balompié</td>
      <td>Spanish Primera División</td>
      <td>Algeria</td>
      <td>Center-mid</td>
    </tr>
    <tr>
      <th>434</th>
      <td>Pablo Sarabia</td>
      <td>Sevilla FC</td>
      <td>Spanish Primera División</td>
      <td>Spain</td>
      <td>Center-mid</td>
    </tr>
    <tr>
      <th>607</th>
      <td>A. Ljajić</td>
      <td>Torino</td>
      <td>Italian Serie A</td>
      <td>Serbia</td>
      <td>Center-mid</td>
    </tr>
    <tr>
      <th>206</th>
      <td>F. Bernardeschi</td>
      <td>Juventus</td>
      <td>Italian Serie A</td>
      <td>Italy</td>
      <td>Forward</td>
    </tr>
  </tbody>
</table>
</div>



```python
#According to EA, the best player in the world
similar_players('Cristiano Ronaldo')
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
      <th>Position</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>L. Suárez</td>
      <td>FC Barcelona</td>
      <td>Spanish Primera División</td>
      <td>Uruguay</td>
      <td>Forward</td>
    </tr>
    <tr>
      <th>5</th>
      <td>R. Lewandowski</td>
      <td>FC Bayern Munich</td>
      <td>German Bundesliga</td>
      <td>Poland</td>
      <td>Forward</td>
    </tr>
    <tr>
      <th>16</th>
      <td>S. Agüero</td>
      <td>Manchester City</td>
      <td>English Premier League</td>
      <td>Argentina</td>
      <td>Forward</td>
    </tr>
    <tr>
      <th>1</th>
      <td>L. Messi</td>
      <td>FC Barcelona</td>
      <td>Spanish Primera División</td>
      <td>Argentina</td>
      <td>Forward</td>
    </tr>
    <tr>
      <th>13</th>
      <td>A. Sánchez</td>
      <td>Arsenal</td>
      <td>English Premier League</td>
      <td>Chile</td>
      <td>Forward</td>
    </tr>
  </tbody>
</table>
</div>



```python
#American GK, hero of the 2016 WC
similar_players('T. Howard')
```


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>club</th>
      <th>league</th>
      <th>nationality</th>
      <th>Position</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>983</th>
      <td>D. Benaglio</td>
      <td>AS Monaco</td>
      <td>French Ligue 1</td>
      <td>Switzerland</td>
      <td>GK</td>
    </tr>
    <tr>
      <th>673</th>
      <td>I. Akinfeev</td>
      <td>CSKA Moscow</td>
      <td>Russian Premier League</td>
      <td>Russia</td>
      <td>GK</td>
    </tr>
    <tr>
      <th>680</th>
      <td>G. Ochoa</td>
      <td>Standard de Liège</td>
      <td>Belgian First Division A</td>
      <td>Mexico</td>
      <td>GK</td>
    </tr>
    <tr>
      <th>384</th>
      <td>C. Carrasso</td>
      <td>Galatasaray SK</td>
      <td>Turkish Süper Lig</td>
      <td>France</td>
      <td>GK</td>
    </tr>
    <tr>
      <th>872</th>
      <td>N. Guzmán</td>
      <td>Tigres U.A.N.L.</td>
      <td>Mexican Liga MX</td>
      <td>Argentina</td>
      <td>GK</td>
    </tr>
  </tbody>
</table>
</div>


While not perfect, this is a fun function that can help fans discover new players. For example, next time the EPL is on TV, I'll keep a lookout for Davinson Sánchez, a Colombian CB for Tottenham Hotspur, since he apparently is a top match for my favorite defender, John Brooks.
