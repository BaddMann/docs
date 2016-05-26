![](https://github.com/apfelwerk/Zentral/wiki/images/Zentral_base_RGB.png)
## Zentral Tutorial - Info Overview and deep dive

1) Look to a 10 min walk thru screencast from the Mac Brains - [Hack The Mac contest] (<https://www.youtube.com/watch?v=bDWFRntc2eQ&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=1) in November 2015. In case you want to catch up with basic facts about *osquery, Santa, Zentral*

2) To get you started with Zentral we recommend [Docker](<https://docs.docker.com>).

3) A *docker-compose* file is provided with Zentral sourcem - the "How to..." is covered well in our video tutorial.

4) With docker-compose you can start Zentral in a multi container setup almost instantly.

5) The Docker compose-setup is ready for testing, development and production* (of course some  use Docker already successful in prod).

6) What if you're somehow not feeling safe using Docker in production? Please note Zentral is just a regular Django app - it's deployment is as simple and/or involved like any other Django app.


### Video Tutorials

*Basic tutorial facts:*

```
- Running time: 59:53
- 17 Episodes
- Average length per tutorial: ~ 3-4 min
```

A multitude of things in IT are way easier to follow visually along with documentation.
We hope to help you get started, stay up date with Zentral, know about accompanied technologies - with a step by step Video Tutorial - as a base component of this tutorial. We think it's exciting to watch a full walk thru, at least will help you for to explore osquery /  santa quickly and hope very much to get you started with Zentral in the future.

If you can't wait any longer [start a 17 episode video tutorial here](<https://www.youtube.com/watch?v=gmF0jaDbZ7o&list=PLW2WReANATsX63zhrFErjScIy-HWEjXWf>) - please don't forget to review our summary section below, especially if you need to jump into specific topics like osquery or client enrollment.


##### Pre-requisites
- Prepare a 3rd party Signed TLS/SSL certificate (mandatory) - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#00](<https://www.youtube.com/watch?v=01J3HPrZ-04>)

##### Installation / Setup

- Install Docker and docker-compose - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#01](https://www.youtube.com/watch?v=gmF0jaDbZ7o)
- Install Zentral with docker-compose - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#01](https://www.youtube.com/watch?v=gmF0jaDbZ7o)
- Update Zentral with git and docker-compose - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#15](<https://www.youtube.com/watch?v=_G2LH00sJFs>)

##### Setup / Enroll

- Enroll a client with osquery to Zentral - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#03](<https://www.youtube.com/watch?v=F1P0NTVLJqQ>)
- Enroll a client with Google Santa to Zentral - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#04](<https://www.youtube.com/watch?v=IKgH8WWJTaA>)
- Enroll a client with Munki to Zentral - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#02](<https://www.youtube.com/watch?v=LT7-nU0JwvE>)

##### Setup / Integrations

- Integrate with Sal - a popular munki Dashboard - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#09](<https://www.youtube.com/watch?v=E3rJ73V8WsU>), [Video \#10](<https://www.youtube.com/watch?v=C7W0v94Pv74>)
- Integrate with Watchman Monitoring - a great tool to detect common client issues - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#11](<https://www.youtube.com/watch?v=IPL03ebYcd4>)
- Integrate with JAMF CasperSuite - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#12](<https://www.youtube.com/watch?v=CoCZ7nK3UFA>)

##### Inventory / BusinessUnits

- Merge existing BusinessUnits into a "Meta BusinessUnit" - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#10](<https://www.youtube.com/watch?v=C7W0v94Pv74>)
- Activate API Enrollment for use with osquery, Santa and Munki - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#02](<https://www.youtube.com/watch?v=LT7-nU0JwvE>)
- Review event details from the client - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#02](<https://www.youtube.com/watch?v=LT7-nU0JwvE>)

##### osquery

- use ad hoc queries via distributed queries - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#03](<https://www.youtube.com/watch?v=F1P0NTVLJqQ>)
- setup a minimalistic `osquery only` Zentral TLS server - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#16](<https://www.youtube.com/watch?v=6SvnfmncrD4>)

##### Setup / Probes

- Setup Santa, create a Probe to blacklist and notify - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#06](<https://www.youtube.com/watch?v=2YEdNs6VwTs>), [Video \#07](<https://www.youtube.com/watch?v=8o8EwaaHOrU>)
- Detect and block evil Ransomeware with Santa, osquery and Zentral - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#13](<https://www.youtube.com/watch?v=GzZewrXbO-s>)

##### Setup / Actions

- setup Slack integration with WebHooks - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#05](<https://www.youtube.com/watch?v=YSjdgO7p2mU>)
- create custom links to external web services - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#08](<https://www.youtube.com/watch?v=aVBGjtuy6EU>)
- use template overwrites to MunkireportPHP and other systems - [Details](https://github.com/zentralopensource/docs/blob/master/zentral-tutorial-ref.md) and [Video \#14](<https://www.youtube.com/watch?v=BfTteF2Y62A>)
