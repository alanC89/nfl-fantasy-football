=============================================================================
# This guide will get you started pulling data from your Yahoo Fantasy Football League
=============================================================================

Please use this data to make some slick visualizations of just how awesome,
balanced, or powerful your team and/or league is!

Data pulled from the Yahoo API is in the form of JSON. You can utilize the files
in this format if you feel comfortable working with it, or you can utilize the
Data Transformation script (`Data_Transformations.py`) or the Jupyter Notebook
to convert some of the data into a more friendly CSV.

Once the data is output in .csv format you can then use those files in Excel,
Tableau, or with any visualization package available in Python.

There is a `Data Analysis` folder with some beginnings of automated
charts that can be created in Seaborn. This is obviously much easier and faster
in a program like Excel or Tableau, but if you're like me and like clicking one
button to extract, parse, compile, sort, and export my data, then this tool was
made for you! (*Note that data viz is done in Jupyter Notebooks for ease of
displaying the graphs inline)*

In order for this whole thing to run correctly, you will need to update a few
key scripts, data files, and get your Yahoo API authorization
tokens (very easy):

It may be easier to install a text editor like Atom to do all of these next
steps in one application...that's up to you. You can easily edit JSON files by
changing the extension from .json to .txt, make the desired edits, and change
the extension back to .json.

# Requirements:

  - Python 3
  - IDE/text editor of your choice
  - Jupyter Notebooks (for data visualizations)
  - Knowledge/experience running and executing Python (.py) scripts in and IDE
    or command line
  - Yahoo Developer Account _(process outlined below)_
  - Libraries needed
    - pandas (https://pandas.pydata.org/)
    - yahoo_oauth (https://pypi.org/project/yahoo_oauth/)
    - datetime & json (bundled with Python)

    *If using the visualizations in `./Data Analysis/`*
    - Seaborn (https://seaborn.pydata.org/)
    - Numpy (http://www.numpy.org/)
    - Matplotlib (https://matplotlib.org/)

# League Details

  - This script assumes a *16-week schedule*, including playoffs. If your league
    differs, then you'll need to change the details in the
    `league_info_form.txt` file in the `Initial_Setup` folder.
  - In the same vein, this assumes a *12-team league*.
  - Update your roster position details as well in the `league_info_form.txt`
    file. Default is QB,WR1,WR2,WR3,RB1,RB2,TE,W/R/T,K,DEF,BN1-6, however
    10 bench spots are shown to account for overflow in either unrostered
    players, or empty roster positions during the waiver/drop/add period each
    week. *As a rule of thumb, add 4 bench ('BN') to your roster here to
    account for the unknown*

# Background Information

Yahoo has been hellbent on killing their API for the last few years. In an
effort to try and gather some raw data from my fantasy football league I
pieced together 3 different libraries previously used to  access the Yahoo API.
This was a trial and error method since each library hadn't been updated in a
few years, and there were no clear examples of input/output for a fantasy sports
application. The login and authentication process used in these scripts should
be applicable to every other fantasy sport league hosted on Yahoo, as long as
the appropriate Game Key is utilized.

To start the season, you need to update both the Yahoo Game Key and League ID.
In the code you'll see the following notation used to identify your specific
league:

"380.l.XXXXXX"
  - 380 is the NFL Game Key which Yahoo doesn't publish. Use the
    `get_league_info.py` script to find this key each year. This should also
    work to find MLB, NHL, etc Game Keys
  - XXXXXX is the league ID, this is published each year on the on your
    league's unique Yahoo Fantasy Football URL:
    https://football.fantasysports.yahoo.com/f1/XXXXXX

# Process for updating the script with you league information:

1. __Update fantasy_stats.py__
  - 1.1) Find/Replace 'XXXXXX' with your League ID (5 instances)
  - 1.2) Each year, replace Game Key with new Game Key (also, 5 instances)
    - 2018 Game Key for NFL is '380'

2. __Update ./teams/team_numbers.txt__
  - 2.1) For first time users, update the manager's nickname/real name for each
    team. Go to https://football.fantasysports.yahoo.com/f1/XXXXXX/1 to find out
    which manager is considered Team 1 in your league (i.e. replace
    'Manager 1 Name' with 'Steve'). Repeat for each team. Record each manager's
    name in this .txt file accordingly. Add or delete any extra teams as needed,
    as this script was originally written for a 12-team league. Just be sure to
    use the correct formatting (comma at end of line, except for the last line)


3. __Register for Yahoo Developer Network and get your credentials:__
  - 3.1) Register for a  Yahoo account at https://yahoo.com/
  - 3.2) Go to https://developer.yahoo.com/ --> 'My Apps' --> 'YDN Apps'
  - 3.3) On the lefthand panel, click 'Create an App'
  - 3.4) Name your app whatever you please ("Fantasy Football Stats") in the
      'Application Name' block.
  - 3.5) Select the 'Installed Application' option since we're only going to be
      accessing the data from our local machines.
  - 3.6) Under the 'API Permissions' sections select 'Fantasy Sports' and then
      make sure that 'Read' is selected. 'Read/Write' would only be used if you
      wanted to be able to control your league via Python scripting vice just
      reading the data from your league. You can come back and change these
      options in the future.
  - 3.7) You have now successfully created a Yahoo Developer App and you will see
      `App ID`, `Client ID(Consumer Key)`, and `Client Secret(Consumer Secret)`
      with a long string of random letters and numbers. Yahoo will use these
      keys to allow you to access it's API via OAuth 2.0.

4. __Update the Yahoo OAuth files__
  - 4.1) In the folder `./auth/`, open `oauth2yahoo.json` to edit.
  - 4.2) Copy both your `Consumer Key` and `Consumer Secret Key` into the
      respective lines. After you allow access (next step), you will see 'Access
      Key', 'Refresh Token', 'Token Time', and 'Token Type' automatically be
      added to this file. *DO NOT EDIT THIS FILE EVER AGAIN OR YOU WILL BE IN A
      WORLD OF HURT*
  - 4.3) You should now be able to run the script. Upon your first run, you will
      be asked to authorize Read access to your league. After you accept, you
      will be given an access code. Simply copy and paste this code into your
      command line/Jupyter Notebook cell where prompted.
  - *4.4) You can disable this scripts access to your league by going to your
      Yahoo Account Info --> Recent Activity --> Apps Connected to Your Account
      --> 'remove' access to your created app*

5. __Double check team count and season length__
  - 5.1) If you skipped over the `Assumptions` section, then go back a revisit it
    to make sure you have the right number of weeks and teams specified in the
    script.

6. __Have fun exploring the data!__
  - 6.1) Run `fantasy_stats.py` each week to update all data. Out-weeks will
  either display 0's for everything or the exact same data as the most current
  week.


Resources and documentation for future development and data that can be
extracted from the Yahoo API:

https://developer.yahoo.com/fantasysports/guide/

https://developer.yahoo.com/fantasysports/guide/game-resource.html#game-resource-desc

https://developer.yahoo.com/apps/