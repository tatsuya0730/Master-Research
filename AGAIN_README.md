# AGAIN
Affect Game Annotation Dataset
===
Welcome to the AGAIN Affect Game Annotation Dataset! 

The raw dataset includes the 1116 videos and detailed gameplay logs of 124 participants playing 9 games from 3 different genres.
A cleaned and preprocessed dataset is available, including the 995 videos and detailed gameplay logs.

The files for the raw dataset are found in the `raw_data` folder:
`biographical_data.csv` - biographical information about the participants
`raw_annotation.csv` - arousal annotation logs recorded with PAGAN and RankTrace
`raw_data.csv` - gameplaylogs of 1116 gameplays

The files for the cleaned dataset are found in the `clean_data` folder:
`clean_data.csv` - preprocessed datapoints for 995 gameplay sessions with their corresponding arousal values
`outliers.csv` - list of outlier sessions (including the corresponding video name)

The `clean_data.csv` has been normalised on the session level!
For easier use in both the raw and cleaned dataset variables labelled:
	- Control variables (not inteded for machine learning purposes) are prefixed with `[control]`
	- String features are prefixed with `[string]`

Reference:
---
If you use our database, please cite us as follows:
[citation]

Database Properties:
---

**General Properties:**
Number of Elicitors: 9 games (3 genres)
Gameplay/Video duration: 2 min
Annotation Perspective: First-person
Annotation Type: Continuous unbounded
Affective Labels: Arousal


**Raw dataset:**
Number of participant: 124
Number of Gameplay Videos: 1116
Number of Game-telemetry Logs: 1116
Video database size: 37+ hours


**Cleaned dataset:**
Number of participant: 122
Number of Gameplay Videos: 995
Number of Game-telemetry Logs: 995
Video database size: 33+ hours

Game Names:
---
**Racing Games:**
TinyCars - tiny - top-down arcade-style
Solid - solid - first-person rally
ApexSpeed - apex - third-person speed-racer

**Shooter Games:**
Heist! - fps - first-person shooter
Shootout - gallery - shooting gallery
TopDown - topdown - top-down isometric shooter

**Platform Games:**
Run'N'Gun - gun - run-and-gun shooter
Pirates! - platform - Mario-like platformer
Endless - endless - endless runner

Video Database:
---
Entry name: [ParticipantID]_[Game]_[SessionID].webm

Annotation Log:
---
`game` - name of the game played
`video_name` - name of the corresponding video in the database
`readable_time` - realtime human readable format
`epoch` - UNIX timestamp
`time_stamp` - video position
`arousal_value` - annotation value
`validity` - 0 for restarted sessions; 1 for final annotations

Annotations are recorded only when the annotation value changes. The annotation starts and ends with a 0.
To use the annotation logs, order the values by one of the time-based features and resample the annotation trace. The recommended method is padding the values.

Discrepancy Between Game Logs and Annotations:
---
Gameplay is recorded from the beginning of the start-game countdown to the beginning of the end-game countdown.
Videos and Annotations are recorded from the beginning of the start-game countdown to the end of the end-game countdown.

Annotation sessions are approximately 3 seconds longer than gameplay videos.
Recommended usage is to first cut the annotation-lag from the beginning of the annotation session then cut the over-hanging annotation entries.

Game Logs:
---
Each game is recorded with approximately 4Hz (every 250ms). Due to the limitations of the engine, each time windows are not consistent. To address this issue the raw logfiles aggregate ticks of the engine's update loop. The control variable, `engine_tick` shows the length of each time window in terms of the number of update loops. Due to this processing technique, almost all events (apart from some very sparse ones like `player_death`) are continuous values.

Enemies, projectiles, and objects are only recorded when they are visible to the player! Check variables with `visible` in their names to see how many of these elements are logged in a given time. Features encoding bot behaviour show the intensity of the encounters with all visible bot's health, speed, distance, etc. averaged together.

Game Objects:
---
**Racing Games:**
Enemy cars - 4 in each game
Loop - a large loop in the track in Solid Rally
Jump - jump pads in Apex and Tiny Cars
Obstacle - fire-traps in Apex (reset the player position on collision)

**Shooter Games:**
Enemy bots - with assault rifles in TopDown and FPS and guns in Shootout
Desctructables - destroyable objects in TopDown (orange objects)
Health Pickup - restores health in TopDown

**Platform Games:**
Enemy bots
	- WalkingEnemy(_Variant_) - basic enemies in all games (different visual variants named)
	- ShootingEnemy - stationary enemies with assault rifles in Run'N'Gun
	- Obstacle - crates in Endless (functions as a WalkingEnemy)
	- Boss and MiniBoss - more powerful enemies in Run'N'Gun (have two weapons and larger health pool)
		- BossWeapon - weapons attached to Boss units (have to be destroyed before attacking the Boss)
Enemy attacks
	- EnemyMelee - melee attack of basic enemies in Run'N'Gun
	- EnemyProjectile - ShootingEnemy projectiles in Run'N'Gun
	- BossProjectile(_Variant_) - BossWeapon projectiles in Run'N'Gun (different variants named)
		- Bullet - a basic projectile attack
		- Burst -  bullets fired in quick succession in a line
		- Spread -  a volley of bullets fired in a 45-degree angle in front of the boss (only for the main Boss)
Pickups
	- HealthPickup - restores health in Run'N'Gun
	- HealthBoost - adds extra life and increases player size in Pirates!
	- Point - coins in Pirates! and Endless which give the player score
	- SpeedBoost and SlowDown - game scrolling speed modifiers in Endless

General Features
---
General features encode features which are general across all games. These features are prefixed with `[general]`

`time_passed` - time counted from the start of the recording
`input_intensity` - number of keypresses
`input_diversity` - number of unique keypresses
`idle_time` - percentage of time spent without input
`activity` - inverse of idleTime
`movement` - distance travelled + reticle moved (in shooter games)
`score` - player score
`bot_count` - number of bots visible
`bot_movement` - bot distance travelled
`bot_diversity` - number of unique bots visible
`object_intensity` - number of objects of interest (destructables, pickups, loops, speed boosts, etc.)
`object_diversity` - number of unique objects
`event_intensity` - number of events
`event_diversity` - number of unique events


