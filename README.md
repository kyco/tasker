#Tasker Profiles
This is a collection of tasker profiles and tasks built by kyco.

##Prerequisites
You will need to have purchased:

- [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm "Tasker")
- [required by some tasks] [Secure Settings](https://play.google.com/store/apps/details?id=com.intangibleobject.securesettings.plugin "Secure Settings")
- [required by some tasks] [NFC Locale Plugin](https://play.google.com/store/apps/details?id=se.badaccess.locale.nfc "NFC Locale Plugin")
- [required by some tasks] [Locale SendSilentMail Plug-In](https://play.google.com/store/apps/details?id=com.stedo.sendsilentmail "Locale SendSilentMail Plug-In")

##Tasks
###Fetch and read out weather
The task runs when I boot up my phone and the time is between 07:30 and 9:30.
It fetches my weather report for the day and reads it out to me before opening
Play News to read todays news.

Task:

- Uses wunderground ical API to grab weather information
    - http://wunderground.com to find your 5 digit location code (68262 in my case)
    - Replace the 5 digit code with the one you got from wunderground in the first step. HTTP GET
    - http://ical.wunderground.com/auto/ical/global/stations/68262?units=metric
- Strips the output to only store the text description of the current day
- Stores the weather description in a variable and reads it out

###Home Status
This task uses an NFC tag to change the state of the %HOME variable.
This variable is used in place of the location which is inaccurate using NET only and I do not want to drain my battery by checking the GPS location every couple of seconds.

Toggle %HOME between 1 and 0

###Home
This task runs when I am at home. The profile makes use of NET rather than GPS locations to save battery power.
The task is active from 06:00 - 22:00

Task:

- Turns on WiFi
- Sets sound profile to normal
- Sets auto brightness on
- [Optional] my LG G2 gives me the ability to set a minimum brightness level on auto which I have done here. I've set it to 174 == 60%
- Display rotation on
- Removes lock pin (requires [Secure Settings](https://play.google.com/store/apps/details?id=com.intangibleobject.securesettings.plugin "Secure Settings"))

###Home Night
Home at night will run from 22:00 - 06:00 the next day and will change basic settings. This task compliments the Home task

Task:

- Turns on WiFi
- Sets sound profile to vibrate
- Sets auto brightness off
- [Optional] my LG G2 gives me the ability to set a minimum brightness level on auto which I have done here. I've set it to 50 == 0%
- Display rotation off
- Removes lock pin (requires [Secure Settings](https://play.google.com/store/apps/details?id=com.intangibleobject.securesettings.plugin "Secure Settings"))

###Leave Home
When the home is left, tasker must perform certain changes to the settings.
This task gets fired when the NET location changes and is out the "Home" range.

Task:

- Turns off WiFi
- Sets sound profile to normal
- Sets auto brightness on
- [Optional] my LG G2 gives me the ability to set a minimum brightness level on auto which I have done here. I've set it to 174 == 60%
- Display rotation on
- Sets lock pin (requires [Secure Settings](https://play.google.com/store/apps/details?id=com.intangibleobject.securesettings.plugin "Secure Settings"))

###Social Vibrate
When I am currently in messaging app of sorts, every subsequent message I get from any other messaging app need not produce a sound as this can be annoying if I am currently chatting to multiple people.

While I am using the phone, I also don't need sound notifications of anything else happening on my phone.

When I have any social messaging application open, including email, the phone switches to vibrate only.

###Meeting
The meeting profile is triggered with an NFC tag.

Task:

- If %MEETING = 0
	- Vibrate
	- Create a notification with buttons to most used apps in meetings. In my case, calendar, evernote, contacts
	- Create a cancel notifcation for leaving the meeting (not really in use but may be faster than using an NFC tag)
	- Set the variable %MEETING to 1
- If %MEETING = 1
	- Sound profile to loud
	- Ringer volume to 5
	- Remove all notifications I set for the meeting
	- Set the variable %MEETING = 0

###Toggle WiFi Hotspot
Using an NFC sticker on the laptop, it uses Secure Settings to toggle the WiFi Hotspot.
This is very useful when on the go and you find yourself without internet access.

Task:

- Secure Settings
	- Toggle WiFi Hotspot (requires [Secure Settings](https://play.google.com/store/apps/details?id=com.intangibleobject.securesettings.plugin "Secure Settings"))

###Incorrect Password
When the phone gets stolen, there will most likely be an attempt to gain access to the phone.
However they do not know the password code and if they enter the incorrect password 3 times, a photo will be taken, the current location will be acquired and the information will be sent via email to myself with battery information and WiFi connection details.

Task:

 - Take photo using front camera
 - Get GPS Location
 - Set variable %STOLEN to:
	 - Date: %TIME - %DATE
	 - Battery: %BATT%
	 - Location: %LOC
	 - Location Accuracy: %LOCACC
	 - WiFi: %WIFII
 - Send email to predefined email address with the variable %STOLEN & an attachment with %FOTO which is the last photo taken. (requires [Locale SendSilentMail Plug-In](https://play.google.com/store/apps/details?id=com.stedo.sendsilentmail "Locale SendSilentMail Plug-In"))
 - Clear variable %STOLEN for privacy

###Lost SMS
When the phone is lost, you can simply sms the text #FindMe and the phone will change the media volume to max and start playing a song

Task:

 - Set Media Volume to 15
 - Music Play - File
 - Create notification to stop playing music once you have found the phone

###Stolen SMS
See "Incorrect Password" for details.
The profile simply triggers the task when an sms with the text #Stolen is received.

###Driving
Using an NFC sticker in the car, the state of %DRIVING will toggle between 1 and 0.

###Read SMS Driving
If %DRIVING is set to 1, the phone will automatically read out loud any SMS that is received while I am driving.

Task:

 - Say
	 - SMS from %SMSRN. Message: %SMSRB

###Respond To Phone While Driving
If %DRIVING is set to 1 and I get a phone call, the caller will be read out loud, the phonecall will automatically be ended and an SMS will be sent to the person to let them know I am driving and will respond as soon as possible.

Task:

- Say %CNAME is calling
- Wait 5 seconds
- Send SMS to %CNUM
- End Call