---
title: mysql example NbaStats DB Insert Season Stats (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'NbaStats DB Insert Season Stats'


Modules used in program: 
* `import time`
* `import json`
* `import traceback`
* `import requests`
* `import mysql.connector`

## python NbaStats DB Insert Season Stats

Python mysql example: NbaStats DB Insert Season Stats

```python
import mysql.connector
import requests
import traceback
import json
import time

conn = mysql.connector.connect(user='', password='',
								host='localhost', database='nba_stats',
								auth_plugin='mysql_native_password')

curs = conn.cursor()

try:
	print('Retrieving player list...')
	query = 'Select player_id, player_full_name FROM players where player_id not in (Select player_id from season_totals group by player_id);'
	curs.execute(query)
	rows = curs.fetchall()
	print('List retrieved successfully.')
	for i in rows:
		print('Attempting to get data for player %s' % i[1])
		url = 'https://stats.nba.com/stats/playercareerstats?permode=Totals&playerid=%s' % i[0]
		headers = {
					'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
					'Accept-Encoding': 'gzip, deflate',
					'Accept-Language': 'en-US,en;q=0.8',
					'Connection': 'keep-alive',
					'Host': 'stats.nba.com',
					'Upgrade-Insecure-Requests': '1',
					'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36'
					}
		proxies = {
			'http': '',#insert proxy
			'https': ''#insert proxy
		}
		response = requests.get(url, headers=headers, proxies=proxies, verify=False)
		#response = requests.get(url, headers=headers, verify=False)
		data = json.loads(response.text)
		for j in data['resultSets']:
			if j['name'] == "SeasonTotalsRegularSeason":
				if len(j['rowSet']) > 0:
					for k in j['rowSet']:
						print('Inserting data......')
						query_in = """INSERT INTO season_totals (player_id,
							season_year, season_league_id, team_id, team_abbr,
							season_player_age, season_gp, season_gs, season_min,
							season_fgm, season_fga, season_fg_pct, season_fg3m,
							season_fg3a, season_fg3_pct, season_ftm, season_fta,
							season_ft_pct, season_oreb, season_dreb, season_reb,
							season_ast, season_stl, season_blk, season_tov, season_pf,
							season_pts) VALUES %r;""" % (tuple(['0' if type(v)==type(None) else v for v in k]),)
						curs.execute(query_in)
						conn.commit()
		time.sleep(10)


except Exception:
	print('Error occured: ')
	traceback.print_exc()
	conn.rollback()
finally:
	if conn:
		conn.close()

	if curs:
		curs.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
