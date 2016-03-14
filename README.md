# Rails Memory Bloat

This is a gem in two acts:

## ACT I: The Logger

When the gem is added to a rails app, it begins to log information on each request. Specifically, it logs the size of the process and the Active Record instantiation breakdown care of [active-record-instance-count](https://github.com/ruckus/active-record-instance-count).

The logs the gem creates look like this:

```
[2016-03-14 19:04:56 +0100]  INFO --: [Memory Usage] PID: 10730 Project: nil User: nil Mem: 179752.0 Records: (110 | User: 39 | Asset: 24 | Pic: 20 | Playlist: 6 | Track: 5 | Comment: 5 | Forum: 3 | Topic: 3 | Post: 3 | Update: 2); /
[2016-03-14 18:47:16 +0100]  INFO --: [Memory Usage] PID: 9815 Project: nil User: 1 Mem: 297224.0 Records: (103 | Asset: 29 | Pic: 21 | User: 16 | Playlist: 15 | Listen: 10 | Track: 5 | Comment: 5 | Update: 2); /sudara```


### Installation of the logger

Adding this gem to your Gemfile will immediately change your rails logging output, engaging Act I :) 

```ruby
gem 'rails_memory_bloat'
```

The logger requires the [active-record-instance-count](https://github.com/ruckus/active-record-instance-count) gem to gather Active Record information. It also depends on the [GetProcessMem](https://github.com/schneems/get_process_mem) to either efficiently grab memory info from `/proc/#{pid}/status` (or resort to shelling out to `ps`)


## ACT II: The Parser and UI

The gem also contains an executable to parse the logs and generate a graphical and tabular representation of process size over time.

gnuplot is required.


The executable requires gnuplot.


### Tasty Example

![](http://skitch.sudara.at/2016-03-14-c07da.jpg)


## Caveats


### This gem adds latency

We run this code in production, but only for a few hours on one server. The goal is to grab a good set of live production logs. 

In our measurements, 10-20ms of time is added by grabbing the PID and memory info, which we consider unacceptable for constant use in production.

### Doesn't work on Windows

See https://github.com/schneems/get_process_mem/issues/15


## Updating this gem

Bump `lib/rails_memory_bloat/version.rb` and `gem build rails_memory_bloat.gemspec`