# Xgress notes
Sources will not be published for some reasons, but below information can be helpfull to make your own solution.

### Development stack
Backend
- Python 3.9
- Requests
- Mongoengine + Pymongo (raw queries for scrapper)
- Pika for queuing
- Elasticsearch (Full text search was used for portals searching by name)
- Shapely to calculate polygons
- Selenium for updating accounts cookies
- Tornado framework for API

Frontend
- JavaScrypt
- React

Old version of frontend
- Angular

Plugin
- jQuery (IITC)

### Monitoring
- Prometheus
- Grafana
- Telegram for alerting

### Backends
- MongoDB 4.4, 2 instances (Main DB + archive data)
- RabbitMQ 3.8
- Elasticsearch 7
- K8s
- Tor proxies

### Hardware
Dedicated server (last configuration) with:
- AMD Ryzen 9
- 128Gb RAM
- 3Tb NVMe disks
- 1Gbit bandwidth
- 2Tb of traffic per month (inc. backups)

### Workers
- Comm /r/getPlexts: 9 threads * 2 accounts for each thread for rotation (7 should be enough to cover the whole map)
- Tiles /r/getEntities: 3 threads * 2 accounts
- Portals /r/getPortalDetails: 4 threads * 3 accounts

Other APIs (Missions, Score and so on) were called very rare and reused accounts from above APIs.

All queries made through 24 instances of tor proxies

### Feeding workers
Celery framework

- Task scheduler
- Workers splitted by APIs type
- Each worker had own queue in RabbitMQ
- Side tasks like telegram notifications, new portals, spoofers, field size...
- Archiver for moving data to second instance of DB for records older than a month

### Scrapper
- Query only cells that have activity
- Each cell has weight, how frequent worker needs to call it
- Each response has maximum batch size 50 records, do call with the timestamp from the last record from previous batch while don`t get less than 50 or empty response. Do pause for more frequent cell to 20mins
- Based on xgress user usage some cells were prioritized and had lower latency for scrapping
- Rotate accounts to avoid soft bans for frequent calls
- Regualar cell zoom is size of intel cell, but for some regions like Japan it had splitted cells with bigger zoom

### Just numbers
- 700Gb DB size in total, 5kkk records of comm logs (exclude mods data).
- 25k registered users
- 2k active users per day based on unique IP (includes crawlers)
- Last few years about 110kk new records per month, before Prime it was much much bigger
- 1 developer + 1 admin and seller
- Thousands of thanks from our beloved users ❤️

### Reason to terminate the project
It was a hobby project from the beginning and I got a lot of experience out of it. I'm tired of maintaining this and it's been a long journey. I wanted to stop this when the guardian medals disappeared, but decided to keep it because people used this service a lot. 

Finally the game is not the same as it was years ago, in my opinion the redacted one was the best app Niantic made. Hopefully the information above will help create some on-premises solutions for tracking gaming activity and detecting spoofers. I think all our haters are happy to know that the project is gone. Fun fact, some of them used xgress to get the data. 🙂️️️️️️ I wish you great gameplay.

