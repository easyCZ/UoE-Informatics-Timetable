UoE Informatics Timetable
Android Application
===
**1.0 Introduction**

The purpose of this document is to outline the architecture and the design process of an Android application designed for SELP at the University of Edinburgh in 2013.


**2.0 Project Setup and Build**

The project contains two directories, one is actionbarsherlock and the other is SELP3. The first directory is a library used to implement advanced action bar features such as tabs and action bar icon support and is required to build the project. The second folder, SELP3, contains the actual source code for the application.


**2.1 Libraries**

The project uses two libraries outside of native Android 2.3.3 version. The first has already been mentioned, Action Bar Sherlock is an open and free to use implementation of Android’s action bars available in later releases made available for Android 2.2 and above. The second library used is the android support library v4. It provides features such as Fragments for Android APIs.


**2.2 Project setup in Eclipse**

It is important to first set up the Action Bar Sherlock library in the workspace and then import the actual project. A clean workspace is assumed for the instructions on how to build the project. First, import actionbarsherlock as a an Android project from existing source. Tick copy to workspace just to make sure the paths for both the source and the library is the same. Second, import SELP3 as an Android project into Eclipse, ticking copy to workspace. When the workspace build is complete, Eclipse should be highlighting an error. If not then the linking for sherlock has for some reason worked automatically. If not, right click on SELP3 directory in the project explorer and go to Properties. Under the Android section, add our existing project directory actionbarsherlock in the Library section on the bottom and mark it as a library. Rebuilding (Clean-ing) the workspace should remove errors. At this point, it should be able to run the project.


**3.0 Features**

The application provides various features to make creating and managing informatics timetable easy. As a broad outline, the application supports download and extraction of data from XML files containing information about courses, venues and lectures themselves. It is possible to select a custom subset of courses to be tracked and displayed on the main screen of the application to be readily available. Additionally, it is possible to search the complete list of courses provided and display details of each course with information about the course as well as the location and times of each of its lectures. If Google Maps are present on the smartphone, navigation to the location of a lecture can be requested.


**3.1 Downloading Files**

When the application is first launched, it contains no information about any lectures. It is necessary to download the information first and store it in an internal database. The image on the right side (empty space cropped out) is the screen a user will see. There is a note saying that no data are available and points the user to download the data. Selecting the download icon in the action bar, we obtain a download screen letting us download the data. Clicking Download and Parse XML will download, parse and save the data into the database. When done, the user is asked to return to the main screen. On smartphones with newer version the left icon in the action bar can be tapped, in 2.3.3 the hardware button press is required.


**3.2 Selecting Courses**

The application requires some lectures to be added as ‘tracked’ to be shown on the main screen. Selecting the plus icon in the action bar will redirect the user to a screen with a list of courses sorted by year. Clicking the tab or swiping left or right will change the tabs. The labels on the side may not show completely, however, as the user swipes they will slide to the side and appear. Tapping the row or the checkbox will mark the course as ‘tracked’ and will automatically make sure it is displayed on the main screen in the correct day tab. The changes are automatically saved into the database.


**3.3 Displaying lectures**

To display lectures, return back to the main screen. If the courses have lectures data associated with them and the semester matches current semester dates, it will display in the list under the appropriate day tab. Once again swiping left or right will move the user to the next day, or alternatively, tapping the tab will do the same. Courses are listed in order of starting time. If a lecture lasts 2 hours, it will get displayed as two separate entries. Additionally, the correct tab is automatically selected based on the day it is to allow quick lookup in a hurry.


**3.4 Course Detail**
Tapping a lecture entry will show the details for the course and all lectures associated to it. A sample for SELP is displayed on the right. There are two links in the action bar on the right side, they will open a browser to navigate to either the course website (WEB) or the DRPS website. Below the Credit entry, there is a list of all lectures for the course. Tapping on the ‘directions’ icon will attempt to open Google Maps and show the location. If Google Maps is unavailable, regular Google Maps in the browser will be used. Tapping elsewhere in the row will open the university
website with the building.


**3.5 Search**

Tapping the search icon in the action bar will bring up the search feature. Initially the complete list of all courses is displayed. As the user begins to type, the results are dynamically filtered. The match is found either in the course
name or the course acronym. The search is insensitive to upper/lower case and is also able to return results for queries
that appear inside a word, not necessary at the beginning of it. Tapping the row will, again, show the detail screen for the course. It is the same screen as presented above. Pressing the cross will clear the current query from the input field.


**3.6 Settings**

The application contains a settings section where it is possible to manually override the semester setting. The
semester is chosen based on the current month by default, however, overriding this setting will prevent future automatic
detection to happen. Additionally, information about the date of the download is shown.

**3.7 Additional Features**

The application works nicely for both portrait and landscape mode. One feature of Action Bar Sherlock is when viewed in landscape mode, the tabs are actually combined with the action bar to provide more space. Examples of this behaviour is shown below with screenshots.


**4.0 Application structure and design**

The source is partitioned into packages based on the purpose each file has. I will go through the main parts of the application and outline how they all fit together. 


**4.1 Downloading**

Asynchronous task is used to download the xml files with data for the application to work. The task is linked to the interface and updates it to provide progress feedback for the user. The source for the downloading is located in ug3.selp.timetable.service.


**4.2 Parsing**

The parser used in the application is based on org.w3c.dom package containing DOM parsers. Tags are parsed in groups rather than in linear block which simplifies the logic slightly. All parsers used for this application are located in ug3.selp.timetable.parser package.


**4.3 Data Storage**

Firstly, parsed data is stored into SQLite database built into the Android platform. I have used three tables - lectures, courses and venues. Each table has the same columns as there are attributes in the original XML files. On top of that, the courses table contains a column for tracking if a course is selected. The source for database operations is located in ug3.selp.timetable.db. Secondly, user preferences such as semester overriding are saved into SharedPreferences build into Android. These preferences are located in ug3.selp.timetable.service.


**4.4 Models**

Various object models have been created to simplify data manipulation. All models used are located in ug3.selp.timetable.models package.


**4.5 Adapters**
Two types of adapters have been used in this application. The first type is for creating tabs in the application and its source is located in ug3.selp.timetable.tab_adapters. The second type is for ListView adapters and handles listeners and other actions with ListViews. These ArrayAdapters are located in ug3.selp.timetable.adapters.


**4.6 Activities**

Each major part of the application has its own activity. The activities are located in ug3.selp.timetable.


**4.7 Fragments**

Fragments used for showing day and year tab content are located in ug3.selp.timetable.fragments.


**5 Application flow**

The chart below outlines the flow in the application. MainActivity is the core entry point with SearchActivity and SelectActivity both pointing to the DetailActivity. SettingsActivity controls the preferences of the application.
