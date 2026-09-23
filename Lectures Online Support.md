# Lectures Online tech support guide

- https://github.com/cola-laits/lecturecapture
- https://github.com/cola-laits/caption-processor

Lectures Online is an LTI app which provides a bridge between Canvas (which hosts courses) and Opencast (which hosts sets of recordings).  Most comms around this app come from Mike Heidenreich, in the Slack #proj-lectures-online channel.  Watch this channel for conversations.

The heaviest support load for this app comes at the beginnings of semesters, where teachers are requesting LO in their courses and we're trying to set up everything in both Opencast and Canvas.  That's all finished by the first week of classes, and you shouldn't see any of that activity in October.

In October, many questions are likely to be about Opencast; you don't have to get in the middle of those conversations.  If there's a bug report or a feature request in Lectures Online or some of its surrounding infrastructure, and it can wait for a few weeks to get resolved, just work with Mike to write this up as a Github issue and assign it to me.  I'll deal with it when I get back.

## Captioning

The main ongoing process in the middle of the semester is captioning.  Lectures Online hands off videos to a standalone captioning service we host and pulls completed captions back from it into Opencast.  See the SyncCaptioningService job in Lectures Online, which runs at :15 past the hour every hour.

Captioning takes about 5-10 minutes per hour of lecture, so we're running the captioner 24 hours a day for most of Mo-Fr.  We build up captioning work to do over the day, and clear out that work backlog over the night.  We aim for a 24-hour turnaround time on captioning (although we don't always hit that, especially at the end of the week when we've built up our biggest backlog).  https://redash.development.la.utexas.edu/alerts/4 is an alert that we've hit that 24-hour threshold, and you should be subscribed to this alert.  Late in the week, this alert just means that we've fallen behind, but if it persists for more than a night, it may mean that the captioner is broken and you should investigate it.

The captioner runs in the `caption-processor` namespace in Rancher, in the `worker` deployment.

## Other apps

We have a few other apps that are related to video/streaming, and which sometimes get lumped into Lectures Online by people who don't understand our app breakdown.  You may hear about these.

- https://lecturecapture-monitor.development.la.utexas.edu/monitor.html#
  - Security-camera style view of all current classroom recordings
  - Code is at https://github.com/cola-laits/lecturecapture/tree/master/monitor-server

- https://rscmonitor.slackbot.la.utexas.edu/home
  - Security-camera style view of all current streams.  Also posts 'stream is on'/'stream is off' notifications to Slack.
  - https://github.com/cola-laits/rss-monitor

- https://videomanager.la.utexas.edu/
  - Allows upload/download of videos (not stored in Opencast)
  - https://github.com/cola-laits/video_manager