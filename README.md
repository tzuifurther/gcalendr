
<!-- README.md is generated from README.Rmd. Please edit that file -->

# gcalendr <img src='man/figures/logo.png' align="right" height="139" />

<!-- badges: start -->

[![R build
status](https://github.com/andrie/gcalendr/workflows/R-CMD-check/badge.svg)](https://github.com/andrie/gcalendr/actions)
[![Travis build
status](https://travis-ci.org/andrie/gcalendr.svg?branch=master)](https://travis-ci.org/andrie/gcalendr)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/gcalendr)](https://cran.r-project.org/package=gcalendr)
[![Codecov test
coverage](https://codecov.io/gh/andrie/gcalendr/branch/master/graph/badge.svg)](https://codecov.io/gh/andrie/gcalendr?branch=master)
<!-- badges: end -->

This package enables you to read events from google calendar.

## Installation

The package is not yet on CRAN.

You can install the development version of `gcalendr` from github using:

``` r
# install.packages("devtools")
devtools::install_github("andrie/gcalendr")
```

## Example

Use this example to authenticate, list available calendars and view
events:

``` r
library(gcalendr)

## Set up google oauth permissions
## This will prompt you to specify an account
calendar_auth()
```

``` r
## To specify a specific account, provide your account id, typically an email address
calendar_auth("apdevries@gmail.com")

## Retrieve tibble of available calenders
calendar_ids <- calendar_list()
calendar_ids

## Retrieve tibble of events from a specific calendar

my_cal_id <- "apdevries@gmail.com"
events <- calendar_events(my_cal_id, days_in_past = 90, days_in_future = 90)
events
```

## Creating calendar events

It is possible to create calendar events using the Google Calendar API.
In order to do so, the proper scopes must be provided during the
authentication process. By default the scope used in `calendar_auth()`
is `"https://www.googleapis.com/auth/calendar.readonly"` which provide
insufficient privileges for creating an event. If you desire to create
calendar events your authentication should be modified with the scopes
`"https://www.googleapis.com/auth/calendar.events"`. For example:

``` r
calendar_auth(scopes = "https://www.googleapis.com/auth/calendar.events")
```

The minimum required fields to create a calendar event are the calendar
id and the date.

``` r
create_event(my_cal_id, start_time = "2020-01-01")
```

The above will create an all day event of the specified calendar for the
date January 1st, 2020. A function call to create an event with
attendees, a title, and a description would look like the following:

``` r

create_event(
  id = my_cal_id,
  title = "Andrie / Josiah",
  description = "Time to discuss `gcalendr`",
  attendees = c("apdevries@gmail.com", "josiah.parry@gmail.com"),
  start_time = "2020-01-01 09:00:00",
  duration = 60
)
```
