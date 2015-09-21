# Introduction #

This XBMC addon adds another database to XBMC where all watched-states of tv episodes and movies are stored additionaly. This has the advantage, that after renaming or moving the video files the watched state can be restored based on unique identifiers of the media such as imdb number and TheTVDB id, season and episode number.



# When do you Need this addon? #

  * you move/rename your media files often and don't want to lose the watched state.
  * you want to backup watched-information
  * you want to synchronize the watched state with multiple xbmc clients (without using one central database, e.g. for speed reasons)

# Installation #

## Installation using repository ##
By installing the addon with a repository, it is automatically updateded from within xbmc.
  1. Install SuperRepo Repository (if you don't already have it). [http://superrepo.org/get-started/add-the-super-repo-directory-as-source/](http://superrepo.org/get-started/add-the-super-repo-directory-as-source/)
  1. Configure SuperRepo using "SuperRepo All" [http://superrepo.org/get-started/install-addons-using-the-superrepo-org-repository/](http://superrepo.org/get-started/install-addons-using-the-superrepo-org-repository/)
  1. in xbmc, install via System -> Addons -> More Addons -> SuperRepo All -> Addon Depot -> program addons -> WatchedList

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_superrepo.jpg](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_superrepo.jpg)


## Installation without repository ##

I don't advise this method, because you don't get updates which I make quite often due to automatically reported bugs.
  1. Download the zip files for watchedlist from the [source code](http://code.google.com/p/xbmc-addon-service-watchedlist/source/browse/): [beta](http://xbmc-addon-service-watchedlist.googlecode.com/archive/testing.zip) or [stable](http://xbmc-addon-service-watchedlist.googlecode.com/archive/master.zip).
  1. Install them in XBMC ( http://wiki.xbmc.org/index.php?title=HOW-TO:Install_an_Add-on_from_a_zip_file ). If you don't install in XBMC and just extract to your userdata folder, the dependencies are not installed automatically.



# Setup #


## Basic Settings ##
  1. debug mode: Show more notifications, create more entries in xbmc.log
  1. update watched state of movies
  1. update watched state of episodes
  1. autorun: starts with XBMC after a delay time of _x_ minutes.
  1. startup delay (see above _x_). Set the delay time to not disturb directly at startup.
  1. autostart mode: 'one update' only does one full update of the watchedlist and then quits. 'periodic' executes this update in the interval given below. 'no update' does no full update. You have the possibility to run WatchedList in background.
  1. update interval (see above _y_)
  1. follow user ...: Run in background and update the watchedlist database every time the user changes a watched state
  1. progress dialog: Show a progress bar for every full update. No user interaction possible in this time.


![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_settings_basic.jpg](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_settings_basic.jpg)

## Database Settings ##

  1. DB Method: Either use a SQLite db file or a mysql server for the database of this addon. I recommend a mysql database, since there were problems with the SQLite database file consistency since XBMC v13.
  1. DB File: Use a non-default database file (not in [...]/userdata/addon\_data/service.watchedlist)
  1. DB File: Path to the database-file. To add a network path for access from multiple clients you can browse the path in xbmc or edit this value manually in the "%appdata%\XBMC\userdata\addon\_data\service.watchedlist\settings.xml".
  1. DB File: Filename of the database (SQLite .db file)
  1. DB File: Create a zip backup copy (ca. 40 KB) of the database (ca  100 KB) each time before writing it. With this you can restore any state you want, e.g. when you mess with your xbmc-database and too much media is marked as watched. On the contrary the Directory will be filled with new files on every change to the WL-DB (no file Rotation, drive space).
  1. MySQL: IP-Adress of the Server. Make sure, there is a mysql server running. To start, use [this](http://wiki.xbmc.org/index.php?title=MySQL/Setting_up_MySQL) guide.
  1. MySQL: 3306 is the default mysql port. No need to change it.
  1. MySQL: Name of the database. You manually need to create this database on the mysql server (use phpMyAdmin)
  1. MySQL: Username. This user must exist and have all necessary permissions on the database set before
  1. MySQL: password for this user.

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_settings_database.jpg](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_settings_database.jpg)

# Usage #

This addon runs in the background as a service. There is no interaction with the user. The script only gives messages about the current operation

After Startup, the databases are searched for watched-information.

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_start.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_start.png)

Then newly watched movies and tv episodes are written to the addon database (_Remember_):

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_remember.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_remember.png)

After that, media that is not watched in the XBMC database and was previously marked as watched by the addon is again marked as watched in XBMC.


![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_setwatched_episode.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_setwatched_episode.png)


![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_setwatched_movie.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_notification_setwatched_movie.png)


# Database #
## MySQL Database ##
The mySQL database stores all relevant information. It looks like this:

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_mysql_watched_movies.jpg](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_mysql_watched_movies.jpg)

For the description of the tables read below.

## SQLite Database file ##
The SQLite Database file option is no longer recommended. To transfer an existing SQLite WatchedList Database, read below.

The script creates a SQLite database. To view the database I used SQLite Database Browser.

https://sourceforge.net/projects/sqlitebrowser/files/latest/download

This figure shows the SQLite db file with automatically created backup copies.

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_db_folder.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_db_folder.png)

The database has three tables.

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_sqlite_structure.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_sqlite_structure.png)

The first table stores the watched movies by imdb-number. The title column is only for easier user access to the table.

![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_sqlite_watched_movies.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_sqlite_watched_movies.png)

The second table stores watched tv episodes with unique number for the TV Show (this field is called imdbnumber in XBMC, but this is the TheTVDB number). The names of the tv shows are stored in a third table, only for better readability.


![http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_sqlite_watched_episode.png](http://wiki.xbmc-addon-service-watchedlist.googlecode.com/git/images/service_watchedlist_doc_sqlite_watched_episode.png)

## Conversion from SQLite database to mysql database ##

In Version 1.0.0 of this addon, I added the possibility to use a mysql database, which has a few advantages over the SQLite Database file:

  * simultaneous access possible
  * mysql handles all the database access, therefore no permanent backup copying necessary
  * no filesystem access in the addon. Should lead to fewer errors

How to convert the database:

  1. open the old database file in SQLite database browser
  1. File -> Export -> Database to sql file
  1. bring this into the new format. For Replacement, I used Notepad++ in Windows:
    * Replace regular expression "INSERT INTO movie\_watched VALUES\((\d+),(\d+),(\d+),(\d+)," with "INSERT IGNORE INTO movie\_watched VALUES\((\1),(\2),FROM\_UNIXTIME\((\3)\),FROM\_UNIXTIME\((\4)\),"
    * Replace regular expression "INSERT INTO episode\_watched VALUES\((\d+),(\d+),(\d+),(\d+),(\d+),(\d+)\)" with "INSERT IGNORE INTO episode\_watched VALUES\((\1),(\2),(\3),(\4),FROM\_UNIXTIME\((\5)\),FROM\_UNIXTIME\((\6)\)\)"
  1. Save this new sql file
  1. execute the commands in the "INSERT"-commands in the sql query interface in phpmyadmin _after_ the tables were generated at the first start of the watchedlist addon with mysql option enabled.



# Technical Details #

The XBMC Database is queried and updated with JSON-RPC. It should not matter whether you use xbmc mysql or xbmc local database.

# Limitations #

  1. The watched-status is only stored based on imdb/thetvdb number. Different Versions of a movie (DVD, BlueRay, Extended Edition, Directors Cut) are all considered equally watched
  1. This addon is only tested under XBMC Frodo 12.1 but should work on older versions too


## Alternatives ##
  1. Online List of watched movies
    * http://code.google.com/p/xbmc-follwit/ (Last update April 2012, probably not Frodo-compatible)
  1. separate script to scan xbmc library (probably not executable from within xbmc)
    * http://forum.xbmc.org/showthread.php?tid=62874&pid=1280902#pid1280902
  1. other similar addons
    * http://code.google.com/p/xbmc-watched-data/ (defect in Frodo?)
    * http://forum.xbmc.org/showthread.php?tid=129448&pid=1401643#pid1401643 (working in Frodo, a little bug, because comparison of names instead of unique numbers)
  1. Export watched flag in nfo file and Import it back again:
    * http://wiki.xbmc.org/index.php?title=Advancedsettings.xml#.3Cvideolibrary.3E

# Acknowledgement #

This code is based on a more simple python script in this forum thread:
http://forum.xbmc.org/showthread.php?tid=62874
The source code of the addon service.libraryautoupdate was also a great help