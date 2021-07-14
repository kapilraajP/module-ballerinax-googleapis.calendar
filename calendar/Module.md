## Overview
The module provides the capability to programmatically manage events and calendar, CRUD operations on event and calendar operations. Additionally this module provides service account authorization that can provide delegated domain-wide access to GSuite domain. So that GSuite admin can do the operations on behalf of the domain users.

This module supports [Google Calendar API](https://developers.google.com/calendar/api) version V3.
 
## Configuring connector
### Prerequisites
- Google account

### Obtaining tokens
1. Visit [Google API Console](https://console.developers.google.com), click **Create Project**, and follow the wizard to create a new project.
2. Go to **Credentials -> OAuth consent screen**, enter a product name to be shown to users, and click **Save**.
3. On the **Credentials** tab, click **Create credentials** and select **OAuth client ID**. 
4. Select an application type, enter a name for the application, and specify a redirect URI (enter https://developers.google.com/oauthplayground if you want to use [OAuth 2.0 playground](https://developers.google.com/oauthplayground) to receive obtain the access token and refresh token). 
5. Click **Create**. Your client ID and client secret appear. 
6. [Enable Calendar API in your app's Cloud Platform project.](https://developers.google.com/workspace/guides/create-project#enable-api)
7. In a separate browser window or tab, visit [OAuth 2.0 playground](https://developers.google.com/oauthplayground).
8. Click the gear icon in the upper right corner and check the box labeled **Use your own OAuth credentials** (if it isn't already checked) and enter the OAuth2 client ID and OAuth2 client secret you obtained above.
9. Select required Google Calendar scopes, and then click **Authorize APIs**.
10. When you receive your authorization code, click **Exchange authorization code for tokens** to obtain the refresh token and access token. 

## Quickstart

## Create an quick add event
### Step 1: Import the Calendar module
First, import the `ballerinax/googleapis.calendar` module into the Ballerina project.
```ballerina
import ballerinax/googleapis.calendar;
```

### Step 2: Initialize the Calendar Client giving necessary credentials
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

calendar:Client calendarClient = check new (config);
```
Note: Must specify the **Refresh token** (obtained by exchanging the authorization code), **Refresh URL**, the **Client ID** and the **Client secret** obtained in the app creation, when configuring the Calendar connector client.

### Step 3: Set up all the data required to create the quick event
The `quickAddEvent` remote function creates an event. The `calendarId` represents the calendar where the event has to be created and `title` refers the name of the event.

```ballerina
string calendarId = "primary";
string title = "Sample Event";
```

### Step 4: Create the quick add event
The response from `quickAddEvent` is either an Event record or an `error` (if creating the event was unsuccessful).

```ballerina
//Create new quick add event.
calendar:Event|error response = calendarClient->quickAddEvent(calendarId, title);

if (response is calendar:Event) {
    // If successful, log event id
    log:printInfo(response.id);
} else {
    // If unsuccessful
    log:printError("Error: " + response.toString());
}
``` 
## Snippets
- Add a quick event using service account
```ballerina
import ballerinax/googleapis.calendar;

calendar:CalendarConfiguration config = {
    oauth2Config: {
        issuer: <issuer>,
        audience: <audience>,
        customClaims: {"scope": <scope>},
        signatureConfig: {
            config: {
                keyStore: {
                    path: <path>,
                    password: <password>
                },
                keyAlias: <keyAlias>,
                keyPassword: <keyPassword>
            }}
    }
};

calendar:Client calendarClient = check new (config);

string calendarId = <calendarId>;
string title = "Sample Event";
string userAccount = <userEmail>;

calendar:Event response = check calendarClient->quickAddEvent(calendarId, title, userAccount = userAccount);
// If successful
```

### [You can find more samples here](https://github.com/ballerina-platform/module-ballerinax-googleapis.calendar/tree/master/samples)
