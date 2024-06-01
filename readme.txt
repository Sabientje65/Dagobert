App consisting of two services communicating via a messagequeue (or just plain http, whatever).

# 
Service one, Poller, intended to run on an anonymous system in case of IP/account bans, will poll the steam store on the given interval for the configured games
results will be pushed to a messagequeue which is consumed by the 'main' service
Exposes an HTTP api/listens to a message queue for updates to games to monitor, monitoring happens on a per-game level, not on a per-user level
Newly monitored games will *always* fire off an initial request
Should also handle rate limiting etc. if possible, use a service with cheap ip switches (?), go from IP A -> B when rate limit is hit to avoid fuckery, when hit, a log message or something actionable *should* be sent out though

Polling happens in static intervals


#
Service two, PriceMonitor, the pricemonitor will be the first (can add more at a later point in time if necessary) consumer of the poller's results, saves them in a DB (pg or sqlite) for analytical purposes (price graphs etc?), plus sending out alerts.
Exposes an HTTP API + web frontend for letting users configure the interval, price points (maybe trends?) on which they want to be alerted. Alerts should have several levels/types, and multiple possible targets, starting with Pushover, Email, and maybe some other free services
