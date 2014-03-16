#Tasker Profiles
This is a collection of tasker profiles and tasks built by kyco.

##Tasks
###Fetch and read out weather
The task runs when I boot up my phone and the time is between 07:30 and 9:30.
It fetches my weather report for the day and reads it out to me before opening
Play News to read todays news.

- Uses wunderground ical API to grab weather information
    - http://wunderground.com to find your 5 digit location code (68262 in my case)
    - Replace the 5 digit code with the one you got from wunderground in the first step. HTTP GET
    - http://ical.wunderground.com/auto/ical/global/stations/68262?units=metric
- Strips the output to only store the text description of the current day
- Stores the weather description in a variable and reads it out

###Home
This task runs when I am at home. The profile makes use of NET rather than GPS locations to save battery power.
The task is active from 06:00 - 22:00

- Turns on WiFi
- Sets sound profile to normal
- Sets auto brightness on
- [Optional] my LG G2 gives me the ability to set a minimum brightness level on auto which I have done here. I've set it to 174 == 60%
- Display rotation on

###Home Night
Home at night will run from 22:00 - 06:00 the next day and will change basic settings. This task compliments the Home task

- Turns on WiFi
- Sets sound profile to vibrate
- Sets auto brightness off
- [Optional] my LG G2 gives me the ability to set a minimum brightness level on auto which I have done here. I've set it to 50 == 0%
- Display rotation off

###Leave Home
When the home is left, tasker must perform certain changes to the settings.
This task gets fired when the NET location changes and is out the "Home" range.

- Turns off WiFi
- Sets sound profile to normal
- Sets auto brightness on
- [Optional] my LG G2 gives me the ability to set a minimum brightness level on auto which I have done here. I've set it to 174 == 60%
- Display rotation on

###Take Photo
Secure settings for android is required. When the unlock code or pattern has been entered incorrectly twice, a photo will be taken with the front facing camera and will be stored in internal memory
