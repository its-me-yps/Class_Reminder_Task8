1-> This is a Chrome Extension that will send Notification to you, reminding about your upcoming class 15 minutes in advance.  
2-> You can also use this extension to get Information about your upcoming class and also see your Full Timetable.  
  
**HOW TO INSTALL ?**  
  
1. Clone this Repo.  
2. Open chrome://extensions and turn on Developer Mode.
3. Click 'Load unpacked' and chose the Extension Directory.
  
4. As soon as you Load unpack the Extension folder, a **Popup Window** will appear which will ask you to Login in your Pingala Portal and go to student Pre-registration Application Page. It will give you a **Go to Pingala Portal** button which you can click.
  
5. Once you are on the required Page, Click the extension icon on the toolbar of chrome and click **Class Reminder**.  

A **Popup DropBox** will come which will have *4 options*  
  
1. **Upcoming Class**
2. **Check Full TimeTable**
3. **Resolve Issues**           (*Not working yet*)
4. **Update TimeTable**

Click on the **Update TimeTable** button and then you will get a **alert** from the page conforming that TimeTable was updated.  
  
**HOW TO USE ?  (BUTTONS)**
  
1. **Update TimeTable**  
This Button will only update TimeTable if you are logged in into your Pingal Portal and on the *Student Pre-Registration Application* otherwise it will give **alert** informing that you are on *wrong webpage.*  
  
2. **Upcoming Class**  
This Button will give you information about your upcoming class in form of **Notification**. The Notification will give the *title* of upcoming class and tell its *starting time*.  
Otherwise it will Notify that you have *no upcoming class today*.

3. **Check Full TimeTable**  
This Button will simply open a new tab in Chrome which will show your Full TimeTable in *tabular form*.

4. **Resolve Issues**  
To resolve issue of course clashing in Pingala.  
**NOT WORKING YET** .Nothing will happen if you click it.  
  
**Three Types of Automatic Notification**  
After Installing, the Extension can give you three type of automatic notification.  
  
1. Notification about Class 15 minutes in advance with the title of the course. 

2. Urgent Notification about a Class which is less than 15 minutes ahead. ( This will only happen if you open chrome 15 or less minutes before your next class. This will not be given if the first notification has already been given).  

3. No upcoming Class Today Notification.  

**HOW IT WORKS?**  

1. *Update TimeTable*  
On clicking Update TimeTable popup.js send a message to background.js to 'GetTT' which background.js sends to content.js. content.js then checks the validity of website by comparing header id with that of Pingala Student Pre-registration Page and if found false alerts the user about it and then returns midway otherwise content.js extracts information of classes from the table and sends it back to background.js which then stores the data and then calls the setNotification() function.

2. *Check Full TimeTable*  
On clicking Check Full TimeTable popup.js send a message to background.js to 'ShowTT' which then calls the showTT() function. showTT() function creates a HTML page in form of a string from TimeTable Data(Stored) and then opens it in new tab.  

3. *Upcoming Class*  
On clicking Upcoming Class popup.js send a message to background.js to 'upcomingClass' which then calls upcomingClass() function.

4. **setNotification() and cycle of automatic Notifications**  
Once setNotification() is called it finds the appropriate kind of Notification to show to the users and then setTimeout() function is used to call setNotification() again after the class has started so that it can look for the next upcoming Class. This Cycle can only be broken when user quits chrome.  
Even when the Last Class has passed the setNotification() is still called every 1hr because if someone opened their laptop on Monday Night and didn't shut it till Tuesday Morning then also setNotification() must notify him about Tuesday Classes.  
(The TimeTable data is stored as an object where key of object is day and value of each key is an array of elements where each element represents pair of 'start time' and 'title' of classes happening on that day)

5. **What calls setNotification()?**  
Every time Timetable gets Updated including when you first installed extension and whenever chrome is opened, setNotification() is called and the cycle is broken only when you quit Chrome.  

**Few Bugs and Few points**  

1. Since the setNotification() cycle only ends when you quit chrome, you may get multiple *you have no class today messages* after all your classes have ended ( every 1hr ).  

2. If you click on Check Full TimeTable and Upcoming Class without Updating the Timetable even once, the extension will show error.

3. Check your Device setting, Ensure it allows Notifications from chrome. Mac Doesn't :(  

4. If any Update TimeTable is not working then reload Extension and refresh browser. Content Script is injected into website which were opened after the extension was loaded.