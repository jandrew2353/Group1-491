# Data Dictionary, Topic 4: The "Unbeatable" Underdog

One Division I men's college basketball league: 360 teams, 32 conferences, seasons 2015 to 2026. The files follow
the layout of the Kaggle March Machine Learning Mania data. A season is named by the calendar year in which it
ends: season 2026 runs from November 2025 to March 2026.

Team A and Team C are marked in `teams.csv`, column `brief_alias`. The win rates quoted in the project problem count
2026 regular-season games only; conference-tournament games appear both in `regular_season_results.csv.gz` and,
separately, in `conference_tourney_games.csv`.

| File | Rows | One row is |
|---|---|---|
| `teams.csv` | 360 | a team |
| `conferences.csv` | 32 | a conference |
| `team_conferences.csv` | 4,298 | a team's conference in one season |
| `seasons.csv` | 12 | a season |
| `regular_season_results.csv.gz` | 65,670 | a game, written from the winner's side |
| `box_scores.csv.gz` | 131,210 | one team's statistics in one game |
| `conference_tourney_games.csv` | 3,785 | a conference-tournament game |
| `tournament_seeds.csv` | 680 | a team selected for the national tournament (68 per season) |
| `tournament_results.csv` | 670 | a national-tournament game (67 per season) |
| `team_strength_preseason.csv` | 4,298 | a team's preseason rating and poll points in one season |

Each file also comes as `<name>_sample.csv` with its first 1,000 rows. The `.csv.gz` files are gzip-compressed
CSV; `pandas.read_csv` opens them directly.

## How the files connect

```
conferences.conference_id  <--  team_conferences.conference_id, conference_tourney_games.conference_id
teams.team_id  <--  team_conferences.team_id
               <--  regular_season_results.w_team_id / l_team_id
               <--  box_scores.team_id / opp_team_id
               <--  conference_tourney_games.w_team_id / l_team_id
               <--  tournament_seeds.team_id, tournament_results.w_team_id / l_team_id
               <--  team_strength_preseason.team_id
seasons.season <--  season in every other file

a game        = (season, day_num, w_team_id, l_team_id) in regular_season_results
its box score = two rows in box_scores: (season, day_num, team_id = w_team_id) and (season, day_num, team_id = l_team_id)
```
There is no game id. A team plays at most one game per day, so (season, day_num, team_id) identifies a team's game.

## teams.csv
| Column | Type | Meaning |
|---|---|---|
| team_id | integer, 1101-1460 | team identifier, the same in every file and season (assigned in alphabetical order of name) |
| team_name | text | current name of the school |
| first_season | integer | first season in Division I (1985 means 1985 or earlier) |
| last_season | integer | last season in Division I in this file (2026 = still active) |
| brief_alias | text | `Team A` or `Team C` for the two teams named in the project brief; blank otherwise |

Five schools moved up to Division I during the period; they have no games and no ratings before `first_season`.

## conferences.csv
| Column | Type | Meaning |
|---|---|---|
| conference_id | integer, 1-32 | conference identifier (assigned in alphabetical order of the code) |
| conference_abbrev | text | short code, e.g. `BCC` |
| conference_name | text | full name |

## team_conferences.csv
| Column | Type | Meaning |
|---|---|---|
| season | integer | season |
| team_id | integer | team |
| conference_id | integer | the team's conference in that season |

Conferences have 8 to 16 members. Some schools changed conference between 2023 and 2025.

## seasons.csv
| Column | Type | Meaning |
|---|---|---|
| season | integer, 2015-2026 | season (the year it ends) |
| day_zero | date | calendar date of `day_num = 0` (a Monday); `day_num = 132` is Selection Sunday |
| region_w, region_x, region_y, region_z | text | names of the four national-tournament regions; the first letter of a seed refers to them |

## regular_season_results.csv.gz
All games before Selection Sunday, including conference tournaments. One row per game, written from the winner's side.

| Column | Type | Meaning |
|---|---|---|
| season | integer | season |
| day_num | integer, 0-132 | days since `day_zero` of that season |
| w_team_id | integer | the team that won |
| w_score | integer, points | points scored by the winning team |
| l_team_id | integer | the team that lost |
| l_score | integer, points | points scored by the losing team |
| w_loc | `H` / `A` / `N` | where the winner played: `H` at home, `A` away (the loser was at home), `N` neutral site |
| num_ot | integer, 0-4 | number of overtime periods |

Notes on the calendar:
- Opening day differs by season (day 0 to day 11; day 23 in 2021). There are no games on December 24-26.
- Non-conference games are played from opening day to late December. Multi-team events at neutral sites are
  mostly held around Thanksgiving.
- Conference games run from late December to early March, about two a week (four conferences also play two
  conference games in early December). Conference tournaments are held between early and mid March; most are
  at a neutral site, a few at the higher seed's gym.
- 2019: the export lost the neutral-site flag. Every 2019 neutral-site game is written as if the team listed
  first on the schedule were at home, so `w_loc` is `H` or `A` for every 2019 game.
- 2020: play stopped on March 12, 2020 (day 129). Conference-tournament games after that date were not played,
  and there was no national tournament, so 2020 has no seeds.
- 2021: the season opened on November 25, 2020, and many games were cancelled, so teams played fewer games.
  Most arenas had few or no spectators.

## box_scores.csv.gz
One row per team per regular-season game; both teams of a game have a row. Statistics recorded during the game.

| Column | Type | Meaning |
|---|---|---|
| season, day_num | integer | as above |
| team_id | integer | the team whose statistics are on this row |
| opp_team_id | integer | its opponent in that game |
| fgm, fga | count | field goals made / attempted (all field goals, two- and three-point) |
| fgm3, fga3 | count | three-point field goals made / attempted (included in fgm / fga) |
| ftm, fta | count | free throws made / attempted |
| oreb, dreb | count | offensive / defensive rebounds |
| ast | count | assists |
| tov | count | turnovers |
| stl | count | steals |
| blk | count | blocked shots |
| pf | count | personal fouls |

A team's points in a game are `2*fgm + fgm3 + ftm`.

## conference_tourney_games.csv
| Column | Type | Meaning |
|---|---|---|
| season | integer | season |
| conference_id | integer | conference whose tournament it was |
| day_num | integer | as above |
| w_team_id, l_team_id | integer | winner and loser |

These games also appear in `regular_season_results.csv.gz` (with scores and location).

## tournament_seeds.csv
| Column | Type | Meaning |
|---|---|---|
| season | integer | season (2015-2026, no 2020) |
| seed | text, e.g. `W01`, `X16a` | region letter (W, X, Y, Z; see `seasons.csv`) + seed line 01-16 (01 = strongest); a trailing `a`/`b` marks two teams that play each other for one place |
| team_id | integer | the team |

Sixty-eight teams per season: the 32 conference-tournament winners plus 36 teams chosen by the selection
committee, which also assigns every seed. Seeds for 2026 have been announced.

## tournament_results.csv
National-tournament games, seasons 2015-2025 (no 2020). Same columns as `regular_season_results.csv.gz`; every
game is at a neutral site. Day numbers: 134-135 opening-round games, 136-139 first and second rounds,
143-146 regional semifinals and finals, 152 national semifinals, 154 final. The 2026 tournament has not been played.

## team_strength_preseason.csv
| Column | Type | Meaning |
|---|---|---|
| season | integer | season |
| team_id | integer | team |
| team_name | text | the team's name as printed in that season's preseason publication |
| preseason_rank | integer, 1 = best | rank of the team in that season's preseason rating |
| preseason_rating | points | expected points per game better (+) or worse (-) than an average Division I team on a neutral floor; 0 = average |
| poll_points | integer | points in the preseason media poll (62 voters, 25 points for a first-place vote down to 1 for 25th); 0 = no votes |

Every active team has one row in every season. The rating and the poll are published before the first game and are
not updated during the season.

## What is NOT recorded
- No player data: rosters, minutes, injuries, transfers, coaching changes.
- No in-season ratings or polls.
- No game times, attendance, travel distance, game city, or betting lines.
- No games against teams outside Division I.
- No box scores for national-tournament games.
- No reason why a team chose its non-conference opponents.
