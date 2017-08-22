---
title: EOMS Editing Documentation

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction
This is a documentation for editing the EOMS Android App prototype for handling SMS to interact with the EOMS Trouble Tickets system.

This documentation page was created with [Slate](https://github.com/tripit/slate) and the App was created with [Android Studio](https://developer.android.com/studio/index.html) using the template [Smsbiz](https://github.com/IOException722/SmsBiz).

# Global variable file `EOMS.java`

## Status Strings
```java
private final static String[] statusStrings = {
  "Empty TT",
  "New",
  "Accepting",
  "Working",
  "Finishing",
  "Finished and Closed",
  "Rejected",
  "Closed"
};
```

`private final static String[] statusStrings;`

A string array which contains the available status of a Trouble Ticket. The index of each item should be consistent to the [`int statusId`](#ttClass) of a Trouble Ticket.


## Trouble Ticket Status Icon
`private static int[] statusIconResources;`

A list containing the resources of icon shown in Inbox. Again, the index of the icon should follow the order of the status.

New icon can be added by `File > New > Image Asset`, then putting the corresponding Resource id `R.drawable.xxxxx` in the array.

statusId | String | Icon Resource | Icon
---------|--------|------ |-----
0|Empty TT| /|
1|New|`R.drawable.newtt`|<img src="./images/newtt.png" />
2|Accepting|`R.drawable.waitingtt`|<img src="./images/waitingtt.png" />
3|Working|`R.drawable.workingtt`|<img src="./images/workingtt.png" />
4|Finishing|`R.drawable.waitingtt`|<img src="./images/waitingtt.png" />
5|Finished and Closed|`R.drawable.finishtt`|<img src="./images/finishtt.png" />
6|Rejected|`R.drawable.rejecttt`|<img src="./images/rejecttt.png" />
7|Closed|`R.drawable.closett`|<img src="./images/closett.png" />


## Reasons / Trouble Cause
```java
private final static LinkedHashMap reasonsArray = new LinkedHashMap(){{
  put("Select reason category", new String[]{
    ""
  });
  put("External", new String[]{
    "Power outage",
    "Client-side equipment",
    "Network operator’s equipment",
    "Network operator’s transmission circuit",
    "Others"
  });
}};
```

To add new categories of reason, simply add a new `put(String, new String[]{String, String, ...});` in this function. The First string will be the category, followed by the string array which contains the corresponding reasons.

An `Other` option will cause a user-input field appear to prompt for input for customized reasons.

The categories and reasons shown in the user interface will remain the same order as they are added.

# Activities

## Splash.java
A splash screen that request permission before the app starts.

## Inbox.java
Shows the list existing Trouble Tickets. Trouble Tickets can be filtered their status with the top-left `filterBtn`.

The Title and ID of Trouble Tickets can be searched with the top-right `searchBtn`.

`loadTTList()` will be executed to load and group the SMSs according to their `TTIds` in the format of `001-xxxxxx-yyy`, followed by `parseSMS()` of [`ttClass`](#ttclass) to determine the status, then applying the filter function mentioned above.

Swipe down to reload.

## TTDetail.java
Shows the details of a Trouble Ticket. The interface changes according to the status.

When the Trouble Ticket is waiting user's `Accept` or `Reject`, only the two buttons will appear.

After Accepting the Trouble Ticket and receiving a `successful` message from the system, the status will turn `Working`. A `Finish` Button with a pull down menu for choosing reasons will appear.

In all other cases, there will be no choice of action available.
## SMSRecord.java
Displays a complete SMS record between the user and system response on a particular Trouble Ticket.

# Object Classes
## ttClass.java
Holds the details of Trouble Ticket Object used in this project. It contains a method to parse all sent and received SMSs to a specific Trouble to determine the current status.

In each Current Status, the content of a new SMS will be checked and cause a transition in the status as below:

<table>
  <thead>
    <th>Current id</th>
    <th>Name</th>
    <th>Detects</th>
    <th>Next</th>
  </thead>
  <tr>
    <td rowspan="2">1</td>
    <td rowspan="2">New</td>
    <td>#A#</td>
    <td>2</td>
  </tr>
  <tr>
    <td>#R#</td>
    <td>6</td>
  </tr>
  <tr>
    <td rowspan="3">2</td>
    <td rowspan="3">Accepting</td>
    <td>error</td>
    <td>1</td>
  </tr>
  <tr>
    <td>success</td>
    <td>3</td>
  </tr>
  <tr>
    <td>already closed<br>already finished</td>
    <td>7</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Working</td>
    <td>#F#</td>
    <td>4</td>
  </tr>
  <tr>
    <td rowspan="3">4</td>
    <td rowspan="3">Finishing</td>
    <td>error</td>
    <td>3</td>
  </tr>
  <tr>
    <td>success</td>
    <td>5</td>
  </tr>
  <tr>
    <td>already closed<br>already finished</td>
    <td>7</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Finished and Closed</td>
    <td>/</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>Rejected</td>
    <td>/</td>
    <td></td>
  </tr>
  <tr>
    <td>7</td>
    <td>Closed</td> 
    <td>/</td>
    <td></td>
  </tr>
</table>

## smsClass.java
Holds the raw data of a single SMS.
