# UT Instapoll tech support guide

- https://github.com/cola-laits/polls

You shouldn't have to get into the code of UT Instapoll for anything.  The app is pretty mature and doesn't need to change much.

Support for end-users happens on the utinstapoll@utlists.utexas.edu mailing list, and a non-programmer (Julie or Heather most likely) should be managing that.  Most answers can be found in UT Instapoll's help documents, and people just need help finding them or understanding what they read there.

https://redash.development.la.utexas.edu/alerts/1 reports on stale grade updates, and you should be subscribed to it.  (See below.

## Current support issues

### UT Instapoll loads home page but without styles/javascript

Students have been sending screenshots of UT Instapoll's student home page unstyled (no CSS), and nothing works.  The problem here is that we receive every cookie the browser decides to send to us, and lots of sites out there set cookies that apply to the entire .utexas.edu domain.  (Google Analytics tracking cookies are a big offender here, I think.)  We get all of those cookies and trip Nginx's request-size filter, resulting in https://stackoverflow.com/questions/17524396/400-bad-request-request-header-or-cookie-too-large

Students can fix this by deleting their cookies (for the utexas.edu domain, or just for everything).  If you're in the office and feeling ambitious, invite the student in so we can inspect their cookies and see what other site is abusing so much cookie space.

### Stale grade updates to Canvas

The Redash alert listed above reports on stale grade updates, where a grade update waiting to be passed back to Canvas has been sitting in queue for more than a few hours.  This may trigger and then clear itself in the evenings, especially on days when several extremely large classes have poll results queued up at the same time.  If the alert stays red for more than a few hours, it may mean that the grade-passback engine is stuck.  (This can happen after a network outage.)

If this happens, go into Rancher.  The `ut-instapoll` namespace has a `gradesync` deployment.  Restart that deployment and the problem should fix itself after a bit.  Use the ops dashboard at https://redash.development.la.utexas.edu/dashboards/22-instapoll-ops to watch this start to happen; the 'num stale grade updates' counter should start going down.

### Teachers don't understand what a 'recalled' poll is

When a teacher recalls a poll, the grade for that poll goes away and it's no longer accessible to students or listed as a poll that students have received.  Teachers don't understand this, even though we show a popup when they recall the poll and also have a big red area at the top of the Reports/Grades tab listing their recalled polls and describing what that means.  Most 'bug reports' from teachers in the middle of a semester are just this.

## Useful tools

- `php artisan app:force-course-grade-sync {course_id}` - a user needs course grades to be synced to Canvas right now.  Use should be really rare; useful if e.g. somebody accidentally deletes an assignment and needs course grades populated back into the new one.
- https://polls.la.utexas.edu/admin - simple Filament dashboard for courses.  Useful for looking up a course by teacher.  (Teachers often don't give you enough info in support requests to be able to identify their course.)
