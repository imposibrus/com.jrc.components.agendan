# Agenda
[![Build Status](https://api.travis-ci.org/rschmukler/agenda.png)](http://travis-ci.org/rschmukler/agenda)
[![Code Climate](https://d3s6mut3hikguw.cloudfront.net/github/rschmukler/agenda.png)](https://codeclimate.com/github/rschmukler/agenda/badges)
[![Coverage Status](https://coveralls.io/repos/rschmukler/agenda/badge.png)](https://coveralls.io/r/rschmukler/agenda)

agenda-n is a light-weight job scheduling library for Node.js based on rschmukler's agenda.

It offers:

- Minimal overhead. Agenda aims to keep its code base small.
- Mongo backed persistance layer.
- Scheduling with configurable priority, concurrency, and repeating
- Scheduling via cron or human readable syntax.
- Event backed job queue that you can hook into.

# Installation

Install via NPM

    npm install agenda-n

You will also need a working [mongo](http://www.mongodb.org/) database (2.4+) to point it to.

# Documentation

This package is based on rschmukler's agenda, see docs.

# New features

- every: start and end date support for job executions added, jobs created with every will only run in the date-time interval (if) given. This is helpful as it saves calls to external api's.

```js

// Execute "get results" every 3 minutes on 30th October 2014 between 12:00 and 18:00.
agenda.every(
  '3 minutes', 
  'get results', 
  data, 
  '2014-10-30 12:00',
  '2014-10-30 18:00'
);

agenda.start();
```

- Logs

```js

var Agenda = require('agenda-n');

agenda = new Agenda({
  db: {
    address: 'mongodb://localhost:27017/agenda'
  },
  logs: {
    address: 'mongodb://localhost:27017/agenda',
    collection: 'agendalogs'
  }
});

```

- Configuration file conf.js created

```js

/** Attrs */
var attrs = {
    type : {
        once: 'once',
        single: 'single',
        normal: 'normal'
      } 
};

/** Conf defaults */
var conf = {
    default : {
        lockTime: 10 * 60 * 1000,
        concurrency: 5,
        maxConcurrency: 20,
        processEvery: '5 seconds',
        dbName: 'agendaJobs'
      } 
};

/** Messages */
var msg = {
    fail : {
        invalidRepeat: 'failed to calculate nextRunAt due to invalid repeat interval',
        invalidFormat: 'failed to calculate repeatAt time due to invalid format',
        undefinedJob: 'Undefined job'
      } 
};

```

- single job types: two jobs with same name but different data will generate two separate records.

```js

/** Job 1 */
agenda.every('minute','start update events info', {league: 1});

/** Job 2 */
agenda.every('minute','start update events info', {league: 2});

```