---
title: EOMS API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - java

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction
This is a documentation for the EOMS Android App prototype for handling SMS to interact with the EOMS Trouble Tickets system.

This documentation page was created with [Slate](https://github.com/tripit/slate) and the App was created with [Android Studio](https://developer.android.com/studio/index.html) using the template [Smsbiz](https://github.com/IOException722/SmsBiz).

# `ttClass` - a class that stores TT details
Class Method | Description
-|-
`String getTTId()` | Return the Trouble Ticket ID in format `001-xxxxxx-yyy`
`String getTitle()` | Return the Trouble Ticket Title
`int getStatusId()` | Return an integer that corresponds to different status
`String[] getStrings()` | Return a string array that contains the names of different status
`String getCreateTime()` | Return the create time of the Trouble Ticket in format `yyyy-MM-dd HH:mm:ss`
`String getUpdateTime()` | Return received time of the latest SMS in format `yyyy-MM-dd HH:mm:ss`
`String getOriginator()` | Return the name of the originator of the Trouble Ticket
`ArrayList<smsClass> getSms()` | Return an ArrayList of SMSs of this Trouble Ticket
`void putSms(smsClass sms)` | Add an SMS to the record of this Trouble Ticket
`void parseSms()` | Parse all SMSs under this Trouble Ticket chronologically to determine the current status

Explanation for `putSms()` and `parseSms()` can be found in [Program Flow](#program-flow)

# Android App structure
## App events

> This will be executed when the `Activity` Loads.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
  ...
}
```

An `Activity` represents a single screen in an app.

The `onCreate()` method will be executed when the activity becomes visible to the user. There are various events that will be triggered when the user quits and launches the activity. Ref: [Android App Lifecycle] (https://developer.android.com/guide/components/activities/activity-lifecycle.html)

## Setting Layout (`setLayout()`)

A custom method is implemented to centralized the initiation and layout configuration of visible elements.

All visible elements are subclass of `View`. In most cases, using `setVisibility()` and `setOnClickListener()` is enough to implement the required functionalities in this app.

For component specific methods: [Android UI Guide](https://developer.android.com/guide/topics/ui/index.html)

> Example in `Inbox.java`

```java
private void setLayout() {
  setContentView(R.layout.activity_main);
```

> This selects the `activity_main.xml` in resources as the layout template, followed by:

```java
  searchBtn = (ImageButton) findViewById(R.id.searchBtn);
  if (searchBtn != null) {
    searchBtn.setVisibility(View.VISIBLE);
    searchBtn.setOnClickListener(new View.OnClickListener() {
      @Override
      public void onClick(View view) {
        // When the search button is clicked...
        searchTxt.setVisibility(View.VISIBLE);
        title.setVisibility(View.GONE);
        menu_or_home.setVisibility(View.VISIBLE);
        filterBtn.setVisibility(View.GONE);
        notiswitch.setVisibility(View.GONE);
        menu_or_home.setImageResource(R.drawable.ic_keyboard_backspace_white_24dp);
        searchBtn.setVisibility(View.GONE);
      }
    });
  }
}
```

> which finds the search button then sets its behaviour.

### Selecting layout files
`setContentView(R.layout.activity_main);`

### Locating elements
`searchBtn = (ImageButton) findViewById(R.id.searchBtn);`

### Setting Visibility and behaviour
`searchBtn.setVisibility(View.VISIBLE);`

`searchBtn.setOnClickListener(new View.OnClickListener() {...});`

## Starting new activity with `Intent`
An `Intent` is used to transfer data between activities.




### Building an `Intent`
> Example in `Inbox.java` that sends a TT object to activity `TTDetail` to show its detail.

```java
Intent intent = new Intent(activity, TTDetail.class);
intent.putExtra("TT", TTListById.get(TTIds.get(position)));
startActivity(intent);
```

`Intent intent = new Intent(activity, ActivityName.class);`

Parameter|Description
--|--
`activity` | the Context of the current activity
`ActivityName` | the target activity

### Putting objects in the `Intent`
`intent.putExtra("TT", TTListById.get(TTIds.get(position)));`

<aside class="notice">
The object can be in any type. It will be retrieved by the <code>name</code> as a key when the new activity receives the <code>Intent</code>
</aside>


### Starting the activity
`startActivity(i);`

This initiates the activity.

## Reading the `Intent`
`Intent intent = getIntent();`

### Getting objects
For ordinary types like `int`, `String`, `boolean`, etc., data can be retrieved by:

`boolean foo = intent.getBooleanExtra("bar", defaultValue);`

where `defaultValue` will be assigned to `foo` when `bar` does not exist.

For custom types like `ttClass`, the method `getSerializableExtra` will be used and no default value will be assigned.

`ttClass TT = (ttClass) intent.getSerializableExtra("TT");`

# Program Flow
## Check for permission

# Appearance
## Splash



## Inbox



## TTDetail



## SMSRecord
