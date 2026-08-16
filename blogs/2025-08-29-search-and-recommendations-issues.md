---
title: "Search and Recommendations issues."
url: "https://status.clerk.io/incident/714768"
date: "2025-08-29"
feed_url: "https://status.clerk.io/feed"
---
Postmortem - 2025-08-29 Overview Earlier today, the search, recommendation, and chat services at `api.clerk.io` began malfunctioning, returning an `InternalError` with the message: "An internal error occurred. If you need assistance, please contact our support team." Timeline: All times CEST. - 09:29 - A dependency update from our APM provider (Application Performance Monitoring) was staged for test and deployment.
