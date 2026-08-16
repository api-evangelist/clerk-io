---
title: "Database Zone 2 Down"
url: "https://status.clerk.io/incident/474789"
date: "2024-12-09"
feed_url: "https://status.clerk.io/feed"
---
Recap: At around 15:03 CET one of our database servers began timing out on new connections, leading to API requests for stores on that particular database to experience long wait times which led to system instability. The devops team was notified by automatic monitoring and by 15:10 CET all affected stores had been put into maintenance mode, stabilizing the API operations for all other customers. The issue was diagnosed as a bug in our database software, and the affected machine was patched and re-validated before being put back into production.
