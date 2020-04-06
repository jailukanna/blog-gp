---
title: mysql example migrate to tmp (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'migrate to tmp'

Functions in program: 
* `def copy_table_to_tmp(dump_filename, tmp_table, table, old_columns, new_columns, read_bulk_count=100000,`
* `def _create_tmp_table(table_name, tmp_table):`
* `def course_ids_table():`
* `def _reload_course_id_dict():`
* `def current_ts():`

Modules used in program: 
* `import configparser`
* `import csv`
* `import re`
* `import mysql.connector.errors as errors`
* `import mysql.connector as connector`

## python migrate to tmp

Python mysql example: migrate to tmp

```python
import mysql.connector as connector
import mysql.connector.errors as errors
import re
import csv
from os import path
from datetime import datetime as dt
import configparser


def current_ts():
    return dt.strftime(dt.utcnow(), "%Y-%m-%d %H:%M:%S")


def _reload_course_id_dict():
    cursor.execute("SELECT `course_id_index`,`course_id` FROM `edxapp_event`.`course_ids`;")
    course_id_dict = {}
    for course_id_index, course_id in cursor:
        course_id_dict[course_id] = course_id_index
    return course_id_dict


# check / create course_ids table
def course_ids_table():
    global course_dict
    try:
        cursor.execute("DESCRIBE course_ids;")
        cursor.fetchall()
    except errors.ProgrammingError:
        print("Table `course_ids` doesn't exist, creating")

        # we create this table
        query = """CREATE TABLE `course_ids` (
    `course_id_index` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
    `course_id` varchar(64) NOT NULL,
    `course_id_last_update` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (`course_id_index`),
    UNIQUE KEY `course_id` (`course_id`)
) ENGINE=InnoDB CHARSET=utf8mb4"""

        cursor.execute(query)

    print("Good! Table `course_id` exist, continue")

    # we fill in the course_ids table first
    #    get existing course_id values in course_ids table
    course_dict = _reload_course_id_dict()
    print("Found {} existing course ids in table `course_ids`\n".format(len(course_dict.keys())))

    #    get course_id values
    cursor.execute("SELECT DISTINCT `course_id` FROM `edxapp_event`.`tracking`;")

    course_id_rp = re.compile(r"course-v1:[^+]+\+[^+]+\+[^+]+$")

    course_ids = []

    for course_id, in cursor:

        if not course_id_rp.match(course_id):
            # print('Illegal cid: "{}"'.format(course_id))
            continue
        course_id_index = course_dict.get(course_id)
        if not course_id_index:
            course_ids.append(course_id)

    print("\n{} new course ids to insert".format(len(course_ids)))

    #    fill course_ids table
    if course_ids:
        query = "INSERT INTO course_ids (course_id) VALUES {};".format(
            ','.join(map(lambda x: "(%s)", course_ids)))

        cursor.execute(query, course_ids)
        cnx.commit()

        print("Insert to table `course_ids` complete")

    # reload course_id_dict
    course_dict = _reload_course_id_dict()
    print("Reloaded {} course ids from table `course_ids`\n".format(len(course_dict.keys())))


def _create_tmp_table(table_name, tmp_table):
    """

    :rtype: bool: if tmp table is just created
    """
    # create new table
    if table_name == "tracking":
        create_query = """CREATE TABLE `{}` (
`id` int(11) unsigned NOT NULL AUTO_INCREMENT,
`user_id` int(10) unsigned NOT NULL,
`course_id_index` smallint(5) unsigned DEFAULT NULL,
`time` datetime NOT NULL,
PRIMARY KEY (`id`),
KEY `user_id` (`user_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;""".format(tmp_table)

    elif table_name == "tracking_video":
        create_query = """CREATE TABLE `{}` (
`id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
`user_id` INT(10) UNSIGNED NOT NULL,
`event_type` enum('ping_video','play_video','pause_video','seek_video','stop_video','load_video','video_show_cc_menu','video_hide_cc_menu','speed_change_video','ping_video_dash') DEFAULT NULL,
`time` DATETIME NOT NULL,
`course_id_index` smallint(5) unsigned DEFAULT NULL,
`unit_id` VARCHAR(128) NOT NULL,
`current_time` MEDIUMINT(9) UNSIGNED NOT NULL,
`old_time` MEDIUMINT(9) UNSIGNED NOT NULL,
`new_time` MEDIUMINT(9) UNSIGNED NOT NULL,
PRIMARY KEY (`id`),
KEY `user_id` (`user_id`),
KEY `unit_id` (`unit_id`),
KEY `time` (`time`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;""".format(tmp_table)
    else:
        raise Exception("Wrong table name {}".format(table_name))

    try:
        cursor.execute("DESCRIBE {};".format(tmp_table))
        cursor.fetchall()
        return False
    except errors.ProgrammingError:
        print("Table `{}` doesn't exist, creating".format(tmp_table))

    cursor.execute(create_query)
    print("Table `{}` created".format(tmp_table))
    return True


def copy_table_to_tmp(dump_filename, tmp_table, table, old_columns, new_columns, read_bulk_count=100000,
                      write_bulk_count=50000):
    """
    Actually not only copy, but replace course_id with course_id_index
    :param old_columns: list, the first column must be course_id
    :param new_columns: list, the first column must be course_id_index. The length should match old_columns
    """
    column_length = len(new_columns)
    # prepare columns by quoting them
    old_columns = list(map(lambda x: "`{}`".format(x), old_columns))
    new_columns = list(map(lambda x: "`{}`".format(x), new_columns))

    # initiate with fallback value as 0
    last_old_id = 0
    if not _create_tmp_table(table, tmp_table):
        # table already exists, check existing records
        cursor.execute("SELECT {} FROM {} ORDER BY `id` DESC LIMIT 1".format(','.join(new_columns), tmp_table))
        record = cursor.fetchone()

        if record:
            # we only do the following if there is record in tmp table
            record = list(record)

            course_id_index = record[0]
            for course_id in course_dict:
                this_course_id_index = course_dict[course_id]
                if course_id_index == this_course_id_index:
                    break

            assert course_id

            # check where we left and continue
            record[0] = course_id

            where_clause_list = []
            for column in old_columns:
                where_clause_list.append("{}=%s".format(column))

            query = "SELECT `id` FROM {} WHERE {} ORDER BY `id` LIMIT 1".format(table, ' AND '.join(where_clause_list))

            cursor.execute(query, record)

            try:
                last_old_id = cursor.fetchone()[0]
                print("Last copied record is `id`={} in table `{}`".format(last_old_id, table))
            except TypeError:
                print(
                    "WARNING: last match record not found for table `{}`. Fine, we start from beginning".format(table))

    if path.isfile(dump_filename):
        print("WARNING: {} file exist, overwriting".format(dump_filename))

    where_clause = " WHERE `id`>{}".format(last_old_id) if last_old_id else ""
    # process and dump tracking table data
    cursor.execute("SELECT {} FROM {}{};".format(','.join(old_columns), table, where_clause))

    with open(dump_filename, 'w') as f:
        writer = csv.writer(f)
        write_buf = []
        count = 0
        for row in cursor:
            row = list(row)  # tuple does not support item assignment
            cid_index = course_dict.get(row[0])

            if cid_index:
                row[0] = cid_index
                write_buf.append(row)
            else:
                # Invalid course_id
                continue
            count += 1

            if count % read_bulk_count is 0:
                writer.writerows(write_buf)
                write_buf = []
                print("{} - Dumped {} rows for {}".format(current_ts(), count, table))

        if write_buf:
            writer.writerows(write_buf)
        print("{} - Finished dumping {} rows for {}".format(current_ts(), count, table))

    # fill tracking_tmp table with records

    insert_data_buf = []
    cursor.execute("LOCK TABLES `{}` WRITE;".format(tmp_table))

    with open(dump_filename, 'r') as f:
        reader = csv.reader(f)

        count = 0
        total_count = 0
        for row in reader:
            insert_data_buf.extend(row)
            count += 1
            total_count += 1

            if count >= write_bulk_count:
                assert count == len(
                    insert_data_buf) / column_length  # to make sure nothing wired happened before inserting

                query = "INSERT INTO {} ({}) VALUES {};".format(tmp_table, ','.join(new_columns), ','.join(
                    ["({})".format(','.join(['%s'] * column_length))] * count))
                try:
                    cursor.execute(query, insert_data_buf)
                except errors.ProgrammingError as e:
                    print(query)
                    print(insert_data_buf)
                    raise e

                print("{} - Inserted {} rows to {}".format(current_ts(), total_count, tmp_table))
                count = 0
                insert_data_buf = []

        if count:
            # insert leftover

            # to make sure nothing wired happened before inserting
            assert count == len(insert_data_buf) / column_length

            query = "INSERT INTO {} ({}) VALUES {};".format(tmp_table, ','.join(new_columns), ','.join(
                ["({})".format(','.join(['%s'] * column_length))] * count))
            cursor.execute(query, insert_data_buf)

        print("{} - Finished inserting {} rows to {}".format(current_ts(), total_count, tmp_table))

    cursor.execute("UNLOCK TABLES;")
    cnx.commit()


########################################################################################################################
##################################################         RUN          ################################################
########################################################################################################################
print("Script starts at {}".format(current_ts()))

config = configparser.ConfigParser()
config.read("config.ini")
default_section = config.sections()[0]

config_root = config[default_section]

cnx = connector.connect(user=config_root.get('user'), password=config_root.get('password'),
                        host=config_root.get('host'), database=config_root.get('database'))

cursor = cnx.cursor()
course_dict = {}

course_ids_table()

# tracking_table
copy_table_to_tmp(dump_filename="tracking.txt", tmp_table="tracking_tmp", table="tracking",
                  old_columns=['course_id', 'user_id', 'time'],
                  new_columns=['course_id_index', 'user_id', 'time'], read_bulk_count=1000000, write_bulk_count=50000)

# tracking_video_table
copy_table_to_tmp(dump_filename="tracking_video.txt", tmp_table="tracking_video_tmp", table="tracking_video",
                  old_columns=['course_id', 'user_id', 'event_type', 'time', 'unit_id', 'current_time', 'old_time',
                               'new_time'],
                  new_columns=['course_id_index', 'user_id', 'event_type', 'time', 'unit_id', 'current_time',
                               'old_time', 'new_time'], read_bulk_count=500000, write_bulk_count=20000)

cursor.close()
cnx.close()

print("Script finishes at {}".format(current_ts()))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
