heroku-log-parser
=======

A [syslog (rfc5424)](http://tools.ietf.org/html/rfc5424#section-6) parser written in Ruby and specifically
targeting Heroku's [http log drain](https://devcenter.heroku.com/articles/labs-https-drains).

## Install

Declare `heroku-log-parser` in your `Gemfile`.

```ruby
gem 'heroku-log-parser', :git => 'git@github.com:rwdaigle/heroku-log-parser.git'
```

Run bundler.

```term
$ bundle install
```

## Usage

```ruby
> msg_str = "156 <40>1 2012-11-30T06:45:26+00:00 heroku web.3 d.73ea7440-270a-435a-a0ea-adf50b4e5f5a - Starting process with command `bundle exec rackup config.ru -p 24405`"
> HerokuLogParser.parse(msg_str)

[{:priority=>40,
  :syslog_version=>1,
  :emitted_at=>2012-11-30 06:45:26 UTC,
  :hostname=>"heroku",
  :appname=>"web.3",
  :proc_id=>"d.73ea7440-270a-435a-a0ea-adf50b4e5f5a",
  :msg_id=>nil,
  :structured_data=>nil,
  :message=>
  "Starting process with command `bundle exec rackup config.ru -p 24405`",
  :message_data=>{}}]

> msg_str = '268 <156>1 2012-11-30T06:45:26+00:00 host heroku router - at=error code=H12 desc="Request timeout" method=GET path="/foobar" host=www.example.com request_id=f754d2c6-70fe-4ddb-be9b-29dcab21cc04 fwd="108.162.246.83" dyno=web.8 connect=3ms service=30000ms status=503 bytes=0'
> HerokuLogParser.parse(msg_str)

[{:priority=>156,
:syslog_version=>1,
:emitted_at=>2012-11-30 06:45:26 UTC,
:hostname=>"host",
:appname=>"heroku",
:proc_id=>"router",
:msg_id=>nil,
:structured_data=>nil,
:message=>
"at=error code=H12 desc=\"Request timeout\" method=GET path=\"/foobar\" host=www.example.com request_id=f754d2c6-70fe-4ddb-be9b-29dcab21cc04 fwd=\"108.162.246.83\" dyno=web.8 connect=3ms service=30000ms status=503 bytes=0",
:message_data=>
{"at"=>"error",
  "code"=>"H12",
  "desc"=>"Request timeout",
  "method"=>"GET",
  "path"=>"/foobar",
  "host"=>"www.example.com",
  "request_id"=>"f754d2c6-70fe-4ddb-be9b-29dcab21cc04",
  "fwd"=>"108.162.246.83",
  "dyno"=>"web.8",
  "connect"=>"3ms",
  "service"=>"30000ms",
  "status"=>"503",
  "bytes"=>"0"}}]
```

`HerokuLogParser` is a stateless, regex-based parser that accepts a string of data holding one or more syslog messages
and returns an array of syslog message properties for each message. For those unwilling to read the spec, the
list of syslog tokens is as follows (and is stored in the `HerokuLogParser::SYSLOG_KEYS` array):

```ruby
HerokuLogParser::SYSLOG_KEYS
#=> [:priority, :syslog_version, :emitted_at, :hostname, :appname, :proc_id, :msg_id, :structured_data, :message]
```

## Contributions

* [Ryan Smith](https://github.com/ryandotsmith/) for his work on [l2met](https://github.com/ryandotsmith/l2met) which forms the foundation of heroku-log-parser.

## Issues

Please submit all issues to the project's Github issues.

-- [@rwdaigle](https://twitter.com/rwdaigle)
