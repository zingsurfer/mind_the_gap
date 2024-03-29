# Mind The Gap
January 2018 | Contributors: Autumn Martin

## About
### Intro
Mind The Gap displays available campsites for a date range while minding the gap. By default, this app prevents campsite reservations that will create a one-day gap in campsite reservations, which can be challenging to fill when campers are traveling far and most often want to stay multiple nights. Additionally, Mind The Gap gives you the choice to create whatever gap rule you would like!

Available campsites can be viewed [here](https://mind-the-gap-4-campsites.herokuapp.com/api/v1/campsites/available?start_date=2018-06-04&end_date=2018-06-06) and all camp data can be viewed [here](https://mind-the-gap-4-campsites.herokuapp.com/api/v1/searches?start_date=2018-06-04&end_date=2018-06-06). Instructions for dynamic queries are below. 

### Background
Mind The Gap is a Ruby on Rails API that uses a PostgreSQL database. Its database has two tables, campsites and reservations. Campsites has a one-to-many relationship with reservations, and a reservation belongs to a campsite.

This API has two endpoints,`/api/v1/campsites/available` and `/api/v1/searches`, which both take params, `start_date` and `end_date`.

The first, `/api/v1/campsites/available` returns only available campsite names. This available campsites endpoint is also prepped in the event that an external API call is made to retrieve campsite data in the future.

The second, `/api/v1/searches` returns all campsite and reservation data.

#### Tech Stack
Rails 5.2.1, Ruby 2.5.1, PostgreSQL, ActiveRecord, [Waffle](https://waffle.io/Autumn-Martin/mind_the_gap)

### Assumptions
Assumptions were made about the desired gap rule based on the expected input, expected output, & instructions for this challenge.

I thought it would be best to have a database that an API would serve up data from. I built a database to serve up data to `/api/v1/searches endpoint`, and serialized it to look just like the expected input file. Then I retrieved the serialized result for the `api/v1/campsites?available` endpoint. I considered that this result could be retrieved from an external API endpoint in the future, and prepped the `api/v1/campsites?available` endpoint for that case by using this result to update my database.

Lastly, I considered quite a bit whether to use a new language to me, such as Go or Java, or a tech stack that I was more experienced with. I ultimately decided on the latter due to time restraints.

### Special Considerations
I considered the possibility that different campsite managers might like to use different gap rules. To achieve this, I included an optional parameter where users could choose a different gap rule if they would like.

## Endpoints

## Available Campsite Names Minding a Gap Rule (Gap Rule Is For One-Night by Default)
### GET **/api/v1/campsites/available?start_date={{start_date}}&end_date={{end_date}}**

Required Params:
```
start_date
end_date
```
Optional Params:
```
gap_rule
```

### Example with default gap rule
Request:
```
/api/v1/campsites/available?start_date=2018-06-04&end_date=2018-06-06
Content-Type: application/json
Accept: application/json
```

Response:
```
["Comfy Cabin", "Rickety Cabin", "Cabin in the Woods"]
```

### Example with gap rule of 2
Request:
```
/api/v1/campsites/available?start_date=2018-06-04&end_date=2018-06-06&gap_rule=2
Content-Type: application/json
Accept: application/json
```

Response:
```
["Rustic Cabin", "Cabin in the Woods"]
```

## All campsite & reservation data
### GET **/api/v1/searches?start_date={{start_date}}&end_date={{end_date}}**
Request:
```
GET /api/v1/searches?start_date=2018-06-04&end_date=2018-06-06
Content-Type: application/json
Accept: application/json
```

Response:
```
{
  "search": {
    "startDate": "2018-06-04",
    "endDate": "2018-06-06"
  },
  "campsites": [
    {
      "id": 1,
      "name": "Cozy Cabin"
    },
    {
      "id": 2,
      "name": "Comfy Cabin"
    },
    {
      "id": 3,
      "name": "Rustic Cabin"
    },
    {
      "id": 4,
      "name": "Rickety Cabin"
    },
    {
      "id": 5,
      "name": "Cabin in the Woods"
    }
  ],
  "reservations": [
    {"campsiteId": 1, "startDate": "2018-06-01", "endDate": "2018-06-03"},
    {"campsiteId": 1, "startDate": "2018-06-08", "endDate": "2018-06-10"},
    {"campsiteId": 2, "startDate": "2018-06-01", "endDate": "2018-06-01"},
    {"campsiteId": 2, "startDate": "2018-06-02", "endDate": "2018-06-03"},
    {"campsiteId": 2, "startDate": "2018-06-07", "endDate": "2018-06-09"},
    {"campsiteId": 3, "startDate": "2018-06-01", "endDate": "2018-06-02"},
    {"campsiteId": 3, "startDate": "2018-06-08", "endDate": "2018-06-09"},
    {"campsiteId": 4, "startDate": "2018-06-07", "endDate": "2018-06-10"}
  ]
}
```

## Getting started
### Production
This app is deployed on Heroku. Get started by visiting these links and experimenting with different parameters: 

https://mind-the-gap-4-campsites.herokuapp.com/api/v1/campsites/available?start_date=2018-06-04&end_date=2018-06-06 

https://mind-the-gap-4-campsites.herokuapp.com/api/v1/searches?start_date=2018-06-04&end_date=2018-06-06

### Development
First, clone this repository via `git clone git@github.com:Autumn-Martin/mind_the_gap.git`.

Install the dependencies for this application with [Bundler](http://bundler.io/). Run `bundle` in the CLI.

Run the following to create the database, build tables and columns based the schema, & seed the data:
```
rake db:create
rake db:migrate
rake db:seed
```

Start a server with `rails s`.

Now you may view the app locally in development by visiting `http://localhost:3000/` in your browser.

### Testing

The test suite is created through RSpec. To run this test suite, run `rspec`. Mind the Gap currently maintains 94.81% test coverage according to [SimpleCov](https://github.com/colszowka/simplecov).

Also please note that there is a [test database helper](https://github.com/Autumn-Martin/mind_the_gap/blob/master/spec/test_database_helper.rb), which seeds data purely for test runs. This file can be adjusted if you would like to work with a different data set.
