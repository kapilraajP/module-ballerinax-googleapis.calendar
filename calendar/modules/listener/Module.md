## Overview

This module provides you a notification for the events created, updated and deleted in the calendar.

This module supports [Google Calendar API](https://developers.google.com/calendar/api) version V3.

### Configuring connector
### Prerequisites
- [domain registered](https://developers.google.com/calendar/api/guides/push#registering-your-domain) URL. Callback URl should be registered in th GCP project.

### Obtaining tokens
This process is similar to the default module's process. You can refer steps in default module [documentation](https://docs.central.ballerina.io/ballerinax/googleapis.calendar/1.0.0)

## Quickstart

### Create a listener for new event creation
#### Step 1: Import the Calendar module
First, import the `ballerinax/googleapis.calendar` and `import ballerinax/googleapis.calendar.'listener as listen` modules into the Ballerina project.

```ballerina
import ballerinax/googleapis.calendar;
import ballerinax/googleapis.calendar.'listener as listen;
```

#### Step 2: Initialize the Calendar configuration
You can now enter the credentials in the Calendar client config.
```ballerina
calendar:CalendarConfiguration config = {
    oauth2Config: {
        clientId: <CLIENT_ID>,
        clientSecret: <CLIENT_SECRET>
        refreshToken: <REFRESH_TOKEN>,
        refreshUrl: <REFRESH_URL>,   
    }
};
```

#### Step 3: Initialize the Calendar Listener
Define all the data required to create

```ballerina
int port = 4567;
string calendarId = "primary";
string address = "<call_back url + "/calendar/events">";

listener listen:Listener googleListener = new (port, config, calendarId, address);
```

#### Step 4: Create the listener service
If there is an event created in calendar, log will print the event title

```ballerina
service /calendar on googleListener {
    remote function onNewEvent(calendar:Event event) returns error? {
        log:printInfo("Created new event : ", event);
    }
}
```

## Snippets
- On event update
```ballerina
remote function onEventUpdate(calendar:Event event) returns error? {
  //
}
```

- On event delete
```ballerina
remote function onEventDelete(calendar:Event event) returns error? {
  //
}
```
 
### [You can find more samples here](https://github.com/ballerina-platform/module-ballerinax-googleapis.calendar/tree/master/samples)
