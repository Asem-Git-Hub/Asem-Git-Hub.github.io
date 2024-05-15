---
title: "Browser Artifacts"
classes: wide
header:
  teaser: /assets/images/digital-forensics/Browser_Artifacts_pic/hindsight_gui.png
ribbon:
description: "provide detailed insights into a user’s online activities and behaviors."
categories:
  - Digital_forensics
toc: true
---



# WINDOWS BROWSERS ARTIFACTS 

   Browser artifacts encompass a variety of data types stored by web browsers, including navigation history, bookmarks, and cache data. These artifacts are saved in specific directories within the operating system. While the exact locations and names of these directories vary across different browsers, the types of data stored are generally consistent.
    Here’s an overview of the most common browser artifacts:

   **Navigation History:** Logs the websites a user has visited, which is useful for identifying access to potentially harmful sites.
   **Autocomplete Data:** Provides suggestions based on frequent searches, offering valuable insights when analyzed alongside navigation history.
   **Bookmarks:** Sites saved by the user for quick and easy access.
   **Extensions and Add-ons:** Includes any browser extensions or add-ons installed by the user.
   **Cache:** Stores web content such as images and JavaScript files to speed up website loading times. This data can be particularly valuable for forensic analysis.
   **Logins:** Contains stored login credentials for various websites.
   **Favicons:** Small icons associated with websites, which appear in tabs and bookmarks, providing additional information about user visits.
   **Browser Sessions:** Information related to open browser sessions.
   **Downloads:** Keeps records of files downloaded via the browser.
   **Form Data:** Information entered into web forms, saved to facilitate future autofill suggestions.
   **Thumbnails:** Preview images of websites.
   **Custom Dictionary.txt:** Contains words added by the user to the browser's dictionary.
   These artifacts can be crucial in forensic investigations, as they provide detailed insights into a user’s online activities and behaviors.


## GOOGLE CHROME ARTIFACTS 
  Google Chrome stores user profiles in specific locations based on the operating system: 

  • **Linux:** `~/.config/google-chrome/`
  • **Windows:** `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\`
  • **MacOS:** `/Users/$USER/Library/Application Support/Google/Chrome/`

   Within these directories, most user data is typically found in the Default/ or ChromeDefaultData/ folders. The following files contain significant data:
   History: Stores URLs, downloads, and search keywords. On Windows, the ChromeHistoryView tool can be used to read this history. The "Transition Type" column provides details such as user clicks on links, typed URLs, form submissions, and page reloads.

   ![error](/assets/images/digital-forensics/Browser_Artifacts_pic/ChromeHistoryView.png)


 
   **Cookies:** Stores cookie data. The ChromeCookiesView tool can be used to inspect these files.
    `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\Default\Cookies`
    `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Cookies`

   **Cache:** Contains cached data. Windows users can use ChromeCacheView to inspect this information.
    `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Cache`
    `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\ChromeDefaultData\Cache`

   **Bookmarks:** Contains user bookmarks.
    `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Bookmarks`
    `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Bookmarks`

   **Web Data:** Stores form history data.
   
   **Favicons:** Stores website favicons.
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Favicons`
       `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Favicons`

   **Login Data:** Contains login credentials, including usernames and passwords.
       `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Login Data`

   **Current Session/Current Tabs:** Stores data about the current browsing session and open tabs.
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Current Session`
       `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Current Session`
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Current Tabs`
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\ChromeDefaultData\Current Tabs`

   **Last Session/Last Tabs:** Contains information about the sites that were active during the last session before Chrome was closed.
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Last Session`
       `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Last Session`
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Last Tabs`
       `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Last Tabs`

   **Extensions:** Stores data for browser extensions and addons.
       `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\Default\Extensions\`
       `C:\Users\XXX\AppData\Local\Google\Chrome\UserData\ChromeDefaultData\Extensions\`

   **Thumbnails:** Contains website thumbnails.
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Top Sites`
       `C:\Users\XXX\AppData\Local\Google\Chrome\User Data\Default\Thumbnails` (Older versions)

   **Preferences:** A file that contains a wealth of information, including settings for plugins, extensions, pop-ups, notifications, and more.
   These files are crucial for understanding user activity and behavior, making them valuable for forensic investigations.

### HINDSIGHT TOOL 

   Hindsight is a free tool designed for analyzing web artifacts. Initially focused on the browsing history of the Google Chrome web browser, it has since expanded to support other Chromium-based applications, with more support expected in the future. Hindsight can parse a variety of web artifacts, including:

   - URLs
   - Download history
   - Cache records
   - Bookmarks
   - Autofill records
   - Saved passwords
   - Preferences
   - Browser extensions
   - HTTP cookies
   - Local Storage records (HTML5 cookies)
   - Once data is extracted from each file, Hindsight correlates it with data from other history files and organizes it into a timeline.


Hindsight features a simple web user interface. To start it, run "hindsight_gui.py" (or on Windows, the packaged "hindsight_gui.exe") and visit `http://localhost:8080` in a web browser.

  ![error](/assets/images/digital-forensics/Browser_Artifacts_pic/hindsight_gui.png)

The only field you are required to complete is the "Profile Path". This is where you specify the location of the Chrome profile you want to analyze. The default profile paths for different operating systems are listed at the bottom of the page. Once you have entered the profile path, click "Run".
 
  ![error](/assets/images/digital-forensics/Browser_Artifacts_pic/Profile_Path.png)

  ![error](/assets/images/digital-forensics/Browser_Artifacts_pic/Results.png)

you'll be taken to the results page in where you can save the results to a spreadsheet (or other formats).

  ![error](/assets/images/digital-forensics/Browser_Artifacts_pic/spreadsheet.png)


## FIREFOX ARTIFACT
  
  Firefox organizes user data within profiles, stored in specific locations based on the operating system:
    • **Linux:**   `~/.mozilla/firefox/`
    • **MacOS:**   `/Users/$USER/Library/Application Support/Firefox/Profiles/`
    • **Windows:** `%userprofile%\AppData\Roaming\Mozilla\Firefox\Profiles\`
 
  Within each profile folder, several important files can be found:
  
  **places.sqlite:** Stores history, bookmarks, and downloads. Tools like BrowsingHistoryView on Windows can access the history data. Use specific SQL queries to extract history and downloads information.

  **bookmarkbackups:** Contains backups of bookmarks.
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\bookmarkbackup\`
  
  **formhistory.sqlite:** Stores web form data.
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\formhistory.sqlite`
  
  **handlers.json:** Manages protocol handlers.
  
  **persdict.dat:** Contains custom dictionary words.
  
  **addons.json and extensions.sqlite:** Hold information on installed add-ons and extensions.
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\addons.sqlite`
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\extensions.sqlite`

  **cookies.sqlite:** Stores cookies, and can be inspected using MZCookiesView on Windows.
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\cookies.sqlite`

  **cache2/entries or startupCache:** Contains cache data, which can be accessed through tools like MozillaCacheView.
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\cookies.sqlite`

  **favicons.sqlite:** Stores favicons.
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\favicons.sqlite`

  **prefs.js:** Contains user settings and preferences.

  **downloads.sqlite:** An older downloads database, now integrated into places.sqlite.
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\downloads.sqlite`

  **thumbnails:** Stores website thumbnails.
    `C:\Users\XXX\AppData\Local\Mozilla\Firefox\Profiles\[profileID].default\thumbnails`
  
  **logins.json:** Contains encrypted login information.
   Logins:
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\logins.json`
   Passwords: 
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\key4.db`
    `C:\Users\XXX\AppData\Roaming\Mozilla\Firefox\Profiles\[profileID].default\key3.db (Older Version)`

   **key4.db or key3.db:** Stores encryption keys for securing sensitive information.
   These files are crucial for analyzing user activity and behaviors in a forensic investigation.



## INTERNET EXPLORER ARTIFACTS 

  Internet Explorer 11 organizes its data and metadata across various locations to facilitate easy access and management of stored information and corresponding details.
  Metadata Storage
  Metadata for Internet Explorer is stored in “%userprofile%\Appdata\Local\Microsoft\Windows\WebCache\WebcacheVX”

  **Cache Inspection**
   The IECacheView tool allows for the inspection of cache data, requiring the folder location where cache data is stored. Cache metadata includes:
   - Filename
   - Directory
   - Access count
   - URL origin
    Timestamps for cache creation, access, modification, and expiry
  
  **Cookies Management**
   Cookies can be examined using IECookiesView. Cookie metadata includes:
    - Names
    - URLs
    - Access counts
    - Various time-related details
   Persistent cookies are stored in 
    `%userprofile%\Appdata\Roaming\Microsoft\Windows\Cookies` 
   while session cookies reside in memory.



  **Download Details**
   Download metadata is accessible through ESEDatabaseView, with specific containers holding data such as:
    - URL
    - File type
    - Download location
   Physical download files can be found in `%userprofile%\Appdata\Roaming\Microsoft\Windows\IEDownloadHistory`.

  **Browsing History**
   Browsing history can be reviewed using BrowsingHistoryView. This tool requires the location of the extracted history files and configuration for Internet Explorer. Browsing history metadata includes:
    - Modification and access times
    - Access counts
History files are located in %userprofile%\Appdata\Local\Microsoft\Windows\History.

  **Typed URLs**
   Typed URLs and their usage timings are stored in the registry under NTUSER.DAT at the following paths:
    - `Software\Microsoft\Internet Explorer\TypedURLs`
    - `Software\Microsoft\Internet Explorer\TypedURLsTime`
   These registry entries track the last 50 URLs entered by the user along with their last input times.
   These data and metadata storage practices allow for detailed analysis and management of user activities, aiding forensic investigations and user data management.


## MICROSOFT EDGE ARTIFACTS 
  Microsoft Edge stores user data in %userprofile%\Appdata\Local\Packages. The paths for various data types are:
   Profile Path: `C:\Users\XX\AppData\Local\Packages\Microsoft.MicrosoftEdge_XXX\AC`
   History, Cookies, and Downloads: `C:\Users\XX\AppData\Local\Microsoft\Windows\WebCache\WebCacheV01.dat`
   Settings, Bookmarks, and Reading List: `C:\Users\XX\AppData\Local\Packages\Microsoft.MicrosoftEdge_XXX\AC\MicrosoftEdge\User\Default\DataStore\Data\nouser1\XXX\DBStore\spartan.edb`
   Cache: `C:\Users\XXX\AppData\Local\Packages\Microsoft.MicrosoftEdge_XXX\AC#!XXX\MicrosoftEdge\Cache`
   Last Active Sessions: `C:\Users\XX\AppData\Local\Packages\Microsoft.MicrosoftEdge_XXX\AC\MicrosoftEdge\User\Default\Recovery\Active`


## SAFARI ARTIFACTS 

  **Safari Data Storage**
   Safari stores its data in the `/Users/$User/Library/Safari` directory. The key files and their contents include:

   **History.db:** Contains the history_visits and history_items tables, which store URLs and visit timestamps. You can query this database using sqlite3.

   **Downloads.plist:** Contains information about downloaded files.

   **Bookmarks.plist:** Stores bookmarked URLs.

   **TopSites.plist:** Lists the most frequently visited sites.

   **Extensions.plist:** Contains a list of installed Safari browser extensions. You can retrieve and parse this information using plutil or pluginkit.

   *UserNotificationPermissions.plist:* Lists domains that are permitted to push notifications. Use plutil to parse this file.

   **LastSession.plist:** Contains information about tabs from the last browsing session. Use plutil to parse this file.
   These files are crucial for analyzing user activity and behavior within Safari, making them valuable for both forensic investigations and user data management.


# TOOLS FOR PERFORMING BROWSER FORENSICS

   Here is a list of tools that can be used to perform browser forensics, along with their primary functions:

   **DB Browser:** Used for opening and analyzing .sqlite files, which are common for storing browser data such as history and bookmarks.

   **Nirsoft Web Browsers Tools:** A collection of tools designed for extracting and viewing various browser artifacts from different web browsers.

   **BrowsingHistoryView:** A tool for viewing the browsing history of multiple web browsers, including Chrome, Firefox, and Internet Explorer.

   **ESEDatabaseView:** Used for viewing and extracting data from Extensible Storage Engine (ESE) databases, commonly used by Internet Explorer.

   **Session History Scrounger:** Specifically for Firefox, it helps in analyzing session history data.

   **Sysinternals Strings:** A tool for searching and extracting readable text strings from binary files, useful for browser artifact analysis.

   **OS Forensics:** A comprehensive forensic suite that includes tools for analyzing browser artifacts among other forensic tasks.

   **Magnet IEF:** A forensic tool that recovers digital evidence from various data sources, including web browsers.

   **Browser History Viewer:** A tool for viewing browser history from different web browsers.

   **Browser History Examiner:** Provides detailed analysis and reporting of browser history data.
   
   **Hindsight:** A tool for analyzing web artifacts from Chrome and Chromium-based browsers, providing a detailed timeline of user activity.

   **libsedb:** A library for accessing and reading Extensible Storage Engine (ESE) database files.

   **Web Browser Addons View:** Used for viewing the list of installed web browser add-ons.

   **The LaZagne Project:** An open-source project that retrieves passwords stored on a local computer, including those from web browsers.

   **firepwd.py:** An open-source tool for decrypting Mozilla-protected passwords.

   **Firefox Search Engine Extractor:** Used for opening and extracting data from search.json.mozlz4 files in Firefox.

   **Firefox Bookmark Backup Reader/Decompressor:** Used for opening and decompressing jsonlz4 files that contain Firefox bookmark backups.

   These tools offer a wide range of capabilities for accessing, analyzing, and extracting data from various browser artifacts, making them essential for conducting thorough browser forensics.








