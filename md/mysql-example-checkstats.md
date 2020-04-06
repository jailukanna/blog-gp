---
title: mysql example checkstats (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'checkstats'

Functions in program: 
* `def position_trunk(position):`
* `def stats_up_to_hundred(stats, add):`
* `def sum_stats_up_to_hundred(value, current, stats_points):`
* `def sum_all_stats(stats_points, stats):`
* `def sum_stats(add, factor, stats, stats_points):`
* `def on_level_up(level, levels, position, stats):`
* `def get_stats_for_level(position, level):`
* `def valid_stats(position, level, stats_points, stats):`
* `def run_check():`

Modules used in program: 
* `import mysql.connector`

## python checkstats

Python mysql example: checkstats

```python
from __future__ import print_function

import mysql.connector

from mysql.connector import errorcode

level_offset = 1

config = {
    'user': 'root',
    'password': '',
    'host': 'localhost',
    'database': 'kicksdb',
    'raise_on_warnings': True,
    'charset': 'latin1'
}

creation_stats = {}
upgrade_stats = {}
auto_stats = {}
special_auto_stats = [ 10, 20, 30, 35, 40, 50, 55 ]

creation_stats[10] = [ 40, 30, 30, 30, 30, 20, 15, 35, 30, 20, 55, 60, 55, 30, 0, 0, 0 ]
creation_stats[20] = [ 40, 35, 30, 25, 30, 25, 20, 20, 15, 30, 60, 65, 55, 30, 0, 0, 0 ]
creation_stats[30] = [ 40, 30, 30, 25, 25, 30, 30, 35, 15, 15, 50, 60, 65, 30, 0, 0, 0 ]

upgrade_stats[11] = [ 0, 0, 0, 7, 7, 0, -10, 7, 7, -10, 0, 0, 0, 0, 0, 0, 0 ]
upgrade_stats[12] = [ 0, 0, 7, 0, 7, 0, -10, 0, 7, -10, 0, 7, 0, 0, 0, 0, 0 ]
upgrade_stats[13] = [ 0, 7, 0, 0, 7, 0, -10, 0, 7, -10, 7, 0, 0, 0, 0, 0, 0 ]
upgrade_stats[21] = [ 0, 0, 7, 0, 7, 0, -10, 0, 0, 7, 0, 7, -10, 0, 0, 0, 0 ]
upgrade_stats[22] = [ 0, 7, 0, 0, 7, 0, -10, -10, 0, 7, 7, 0, 0, 0, 0, 0, 0 ]
upgrade_stats[23] = [ 0, 0, 0, 7, 7, 0, -10, -10, 0, 7, 0, 7, 0, 0, 0, 0, 0 ]
upgrade_stats[24] = [ 0, 7, 0, 0, 0, 7, 0, 7, -10, -10, 0, 0, 0, 7, 0, 0, 0 ]
upgrade_stats[31] = [ 0, 7, 7, 0, 0, 7, 0, -10, -10, 0, 7, 0, 0, 0, 0, 0, 0 ]
upgrade_stats[32] = [ 0, 7, 0, 0, 0, 7, 7, 7, -10, -10, 0, 0, 0, 0, 0, 0, 0 ]
upgrade_stats[33] = [ 0, 7, 0, 0, 0, 0, 7, 0, -10, -10, 0, 0, 7, 7, 0, 0, 0 ]

auto_stats[10] = [ 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[11] = [ 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[12] = [ 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[13] = [ 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[20] = [ 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[21] = [ 1, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[22] = [ 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[23] = [ 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[24] = [ 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[30] = [ 1, 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[31] = [ 1, 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[32] = [ 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0 ]
auto_stats[33] = [ 1, 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ]

invalid_players = []

def run_check():
    print('Checking stats of all players with level', level_offset, 'or greater...')

    try:
        cnx = mysql.connector.connect(**config)
        cursor = cnx.cursor()

        query = (
            'SELECT id, name, position, level, stats_points, stats_running, \
            stats_endurance, stats_agility, stats_ball_control, \
            stats_dribbling, stats_stealing, stats_tackling, stats_heading, \
            stats_short_shots, stats_long_shots, stats_crossing, \
            stats_short_passes, stats_long_passes, stats_marking, \
            stats_goalkeeping, stats_punching, stats_defense \
            FROM characters WHERE level >= ' + str(level_offset)
        )

        cursor.execute(query)

        for (
            playerId, name, position, level, stats_points, stats_running,
            stats_endurance, stats_agility, stats_ball_control,
            stats_dribbling, stats_stealing, stats_tackling, stats_heading,
            stats_short_shots, stats_long_shots, stats_crossing,
            stats_short_passes, stats_long_passes, stats_marking,
            stats_goalkeeping, stats_punching, stats_defense
        ) in cursor:
            validStats = valid_stats(
                position, level, stats_points, [
                    stats_running, stats_endurance, stats_agility,
                    stats_ball_control, stats_dribbling, stats_stealing,
                    stats_tackling, stats_heading, stats_short_shots,
                    stats_long_shots, stats_crossing, stats_short_passes,
                    stats_long_passes, stats_marking, stats_goalkeeping,
                    stats_punching, stats_defense
                ]
            )

            if validStats:
                print('[', playerId, ']', name, ': ok')
            else:
                invalid_players.append({'id': playerId, 'name': name})
                print('[', playerId, ']', name, ': invalid stats')

        print('Done!')
        print('Found', len(invalid_players), 'players with invalid stats:')

        for player in invalid_players:
            print('Invalid player:', player['name'], '[', player['id'], ']')


    except mysql.connector.Error as err:
        if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
            print('Access denied. Invalid user or password.')
        elif err.errno == errorcode.ER_BAD_DB_ERROR:
            print('Database', config['database'], 'does not exist.')
        else:
            print(err)
    finally:
        cursor.close()
        cnx.close()


def valid_stats(position, level, stats_points, stats):
    target = get_stats_for_level(position, level)
    target_total = target['total']
    target_stats = target['stats']

    total_valid = sum_all_stats(stats_points, stats) == target_total
    min_valid = True

    for i in range(1, 17):
        min_valid |= stats[i] >= target_stats[i]

    return total_valid and min_valid


def get_stats_for_level(position, level):
    trunk_position = position_trunk(position)
    stats = list(creation_stats[trunk_position])
    stats_points = 10

    if level > 18:
        stats_points += on_level_up(18, 17, trunk_position, stats)
        stats_points += on_level_up(level, level - 18, position, stats)
    else:
        stats_points += on_level_up(level, level - 1, trunk_position, stats)

    if level >= 18 and position % 10 != 0:
        # apply upgrade stats
        wrapper = [stats_points]
        sum_stats(upgrade_stats[position], 1, stats, wrapper)
        stats_points = wrapper[0]

    return {
        'total': sum_all_stats(stats_points, stats),
        'stats_points': stats_points,
        'stats': stats
    }


def on_level_up(level, levels, position, stats):
    level_from = level - levels;

    # calculate stats points to add
    stats_points = 0

    for i in range(level_from, level):
        cur_level = i + 1
        stats_points += 2 if cur_level in special_auto_stats else 1

    # add auto stats
    wrapper = [stats_points]
    sum_stats(auto_stats[position], levels, stats, wrapper)

    return wrapper[0]


def sum_stats(add, factor, stats, stats_points):
    for i in range(0, 17):
        stats[i] += sum_stats_up_to_hundred(add[i] * factor, stats[i], stats_points)


def sum_all_stats(stats_points, stats):
    result = stats_points

    for stat in stats:
        result += stat

    return result

def sum_stats_up_to_hundred(value, current, stats_points):
    add = stats_up_to_hundred(current, value)
    stats_points[0] += value - add

    return add


def stats_up_to_hundred(stats, add):
    if add < 0:
        return add

    result = 0

    for i in range(0, add):
        result = i

        if stats < 100:
            stats += 1
        else:
            break

    return result


def position_trunk(position):
    return position - (position % 10)


run_check()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
