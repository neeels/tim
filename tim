#!/usr/bin/env python
# coding: utf8
#
# Tim
# ===
# 
# project timer (GUI + cmdline)
# 
# You enter project names and start and stop them.
# Tim will keep track of how much you worked (and when) and how many of these
# worked hours you have already submitted to your money supplier.
# 
# (c) Copyright 2013 by Neels Janosch Hofmeyr <neels@hofmeyr.de> (GPLv3)
# 
# This file is part of Tim project timer.
# 
# Tim project timer is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
# 
# Tim project timer is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with Tim project timer.  If not, see <http://www.gnu.org/licenses/>.

__doc__ = '''Tim project timer.

You enter project names and start and stop them.
Tim will keep track of how much you worked (and when) and how many of these
worked hours you have already submitted to your money supplier.

usage:
  tim systray
  tim [start] <project-name> [<comment>]
  tim [running]
  tim [adjust] [@<started-time>|+<duration>|-<duration>|=<duration>]
  tim stop [<comment>]
  tim totals [-l <name>]
  tim log [-l <name>] [-s <date>] [-d|-w|-m|-y] [-D|-W|-M|-Y] [-n <nr>]
  tim tickoff <hours> [<project-name>] 
  tim tickoff --undo
  tim rename <current-project-name> <new-project-name>
  tim forget <project-name>
  tim remember [<id>] [--as <project-name>] [--add-to <project-name>]


--- tim systray ---
Launch a systray icon. Put this in your window manager's autostart.

--- tim start ---
When you start working on a given project, call 'tim start myprojectname'.
The word 'start' can be omitted once a project name is known.

Project names names can be grouped, simply by inserting dots in the name. Any
number of grouping levels is allowed.

As soon as the project name is known ('tim start mygroup.myname'), you only
need to type the part after the last dot ('tim myname'), as long as it does
not have the same name as any other project in any other group.

--- tim running ---
Show the current timer, if any is running. This is the same as calling 'tim'
without any arguments.

--- tim adjust ---
When a timer is currently running, adjust the recorded time. This always
shifts the started-time so that the desired duration results when counting up
to the current moment in time. The word 'adjust' may be omitted.
  @<started-time>
    I have actually started working on this project at <started-time>.
    Adjust the timing counter for the current project accordingly.
    <started-time> format is 'HH:MM', e.g. '14:59'. Wraps past midnight
    (yesterday's time is used when today's time would be in the future).
      tim @9      # make as if I called 'start' at 9 o'clock this morning
      tim @15:30  # ... at half past three. 
  +<duration>
    Add <duration> to the currently running timer.
    <duration>: [HH][:MM], for example:
      tim +1      # add one hour
      tim +1:30   # add one and a half hours
      tim +1.5    # add one and a half hours (decimal dot)
      tim +:30    # add thirty minutes
  -<duration>
    Subtract <duration>. '- 10' is the same as '-10' and '+ -10'.
  =<duration>
    Set the current counter to exactly <duration> hours.
      tim =1      # make as if I called 'start' one hour ago

--- tim stop ---
Stop the currently running timer. Any arguments after the word 'stop' are
stored as a comment for the hours just worked.

--- tim totals ---
Print the current timing totals of hours spent vs. hours ticked-off on a
per-group and per-project basis.
  -l <name>
    Limit the listing to this project (or group).

--- tim log ---
Show the timings for this week (-W is the default). Options:
  -l <name>
    Limit the listing to this project (or group).
  -s <date>
    Start at <date>, of format 'YYYY-MM-DD_hh:mm', where everything left and
    right of 'hh' may be omitted.
      -s 2013-01-11
        Start at 00:00 on 11th of January, 2013.
      -s 2013-01-11_23:59
        Start at 23:59 on 11th of January, 2013.
      -s 10
        Same as '-s 10:00', start 10 o'clock today
      -s 1_
        Same as '-s 1_0:00', start on the first day of this month
      -s 1-1_
        Same as '-s 1-1_0:00', start on the most recent 1st of January
      -s 3-32_
        Same as '-s 4-1_0:00', start on the most recent 1st of April (wrap!)
      (etc.)
  Time options:
    default: show each timing in hours and minutes
    -d/-w/-m/-y  combine hours for each day/week/month/year
    -D/-W/-M/-Y  start at the beginning of this day/week/month/year
    -t  Don't start at the last beginning of, but one whole time period ago.
        For example, '-t -w' starts seven days ago, instead of last monday;
        '-t -m' starts the same day last month instead of this month's 1st.
    -n <nr>
       Repeat above time period for <nr> times into the past.

--- tim tickoff ---
Record hours that you have already billed your money supplier for. If you are
timing private / hobby projects, your total hours will simply grow. But if you
have an employer, ticking off is useful to keep track of how many hours you
have actually passed on to your employer's accounting. If you are using
tickoff for any given project, you probably want to reach a total of zero for
that project at regular intervals. If you've worked less than you billed, your
total hours become negative.
- If no project name is supplied, the tickoff is chronologically spread across
  all projects, so that the hours worked longest ago are ticked off first.
    tim tickoff 32
- You can tickoff a group (yoyodyne), in which case the hours contained in any
  sub-projects and sub-sub-projects are ticked-off in their chronological
  order (see examples below). You can also tickoff subprojects specifically.
    tim tickoff 32 yoyodyne
- If a group's total is negative, I've already billed hours I haven't worked
  yet; over time, sub-projects of such a group will receive new hours, but the
  negative total of the parent will only be neutralised at the next tickoff
  command. You may call 'tim tickoff 0 mygroupname' to chronologically
  neutralise a negative total with hours worked in sub-projects.
    tim tickoff 0 yoyodyne  # neutralise negative total with sub-projects
- If you want to keep strictly separate project groups, you should take
  care to never omit the group name when calling the tickoff command. If it
  should go wrong one day, you can undo tickoffs one by one with:
    tim tickoff --undo

--- tim rename ---
Rename a project while keeping its hours and logs. This may also move projects
around between groups.
Note that this changes the name in all logs back to the start of time, so the
'tim log' command will not look the same as before.

--- tim forget ---
Remove a given project from accounting. The data will still be sitting in the
database, but it will appear to be completely removed with all its history
when interacting with tim.

--- tim remember ---
Recover a project previously forgotten with 'tim forget'. Run without
arguments to get a listing of IDs for forgotten projects or groups.

--- EXAMPLE ---
  This example shows a sequence of tim commands. Imagine that time passes
  between each pair of start ... stop commands.
  
  tim start foo
  tim stop

  tim start yoyodyne.abc   # start timing for project abc in the group yoyodyne
  tim +1                   # fake starting-time so that I've worked 1 hour now
  tim stop "fix a bug"     # stop timing, record in the log with a message
  tim abc                  # start yoyodyne.abc, known after above 'start'
  tim stop
  tim start yoyodyne.xyz   # new project xyz in the yoyodyne group
  tim abc                  # stop yoyodyne.xyz, start yoyodyne.abc, in one go
  tim stop

  tim log                  # show the log of this week (starting monday)
  tim log -w -n 5          # show last five weeks (five times seven days)
  tim log yoyodyne         # show log for yoyodyne.* projects only

  tim totals               # show the sum of all hours for all projects
  tim tickoff 32 yoyodyne  # I have billed yoyodyne for 32 hours.
  tim totals               # Above 32 hours are now subtracted from the totals.
 
'''

if __name__ == "__main__":
  # run commandline client.

  def usage(errmsg=None):
    print __doc__
    if (errmsg):
      print 'tim:', errmsg
      exit(1)
    exit(0)

  usage()
