---
title: karafka Error committing offsets
date: 2019-10-09 11:27:16
tags: karafka
author: 相飞
comments:
- true
categories:
- karafka

---

### kafka 


####  现象

```
I, [2019-10-09T11:23:08.506232 #9611]  INFO -- : Inline processing of topic data-monitor with 1 messages took 265 ms
I, [2019-10-09T11:23:08.506383 #9611]  INFO -- : 1 messages on data-monitor topic delegated to DataMonitorConsumer
E, [2019-10-09T11:23:08.507763 #9611] ERROR -- : [[devops_server_bigger_group] {}:] Error committing offsets: Kafka::UnknownMemberId
I, [2019-10-09T11:23:08.507862 #9611]  INFO -- : [[devops_server_bigger_group] {}:] Joining group `devops_server_bigger_group`
E, [2019-10-09T11:23:08.508926 #9611] ERROR -- : [[devops_server_bigger_group] {}:] Failed to join group; resetting member id and retrying in 1s...
I, [2019-10-09T11:23:09.509288 #9611]  INFO -- : [[devops_server_bigger_group] {}:] Joining group `devops_server_bigger_group`
I, [2019-10-09T11:23:09.511525 #9611]  INFO -- : [[devops_server_bigger_group] {}:] Joined group `devops_server_bigger_group` with member id `devops_server-799ce8f4-d197-4115-82fa-498ae9ee8a16`


```

#### solution


```

```


