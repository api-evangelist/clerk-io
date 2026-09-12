---
title: "Partial API Outage"
url: "https://status.clerk.io/incident/1046330"
date: "2026-09-02"
feed_url: "https://status.clerk.io/feed"
---
# Incident report: API availability **Date:** 2026-09-02 T 07:05 CEST **Duration:** Approximately 10 minutes **Impact:** Some API requests and background operations were delayed or failed for a subset of stores. This also had a knock-on effect on the API availability for other stores as workers got tied up, until automatic scaling added new servers. ## What happened One of our database servers stopped processing queries after an internal MariaDB/InnoDB synchronization operation became stuck.
