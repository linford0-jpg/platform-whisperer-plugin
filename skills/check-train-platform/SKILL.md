---
name: check-train-platform
description: Find which platform a London train leaves from, and whether it is late. Use when the user asks about a train platform, departures, delays or cancellations at London Euston or London Paddington, asks "which platform", or asks how reliable platform predictions are.
---

Use the Platform Whisperer connector to answer questions about departures and platforms at London stations.

1. If the station is unclear, or the user names another station, call `list_stations`. Only stations marked enabled can be used. If a station is disabled or unknown, say it is not available and do not guess.
2. For "what is leaving soon" call `get_departures` for the station, with an operator filter or time window if the user gave one.
3. For one named train, call `find_train` with the destination, time window or headcode. If more than one train matches, ask which one, or list the candidates with their times.
4. For full detail on one train, call `get_train` with its train id, or its headcode and time.
5. For "how accurate is this", call `get_track_record` and report the accuracy for the relevant number of minutes before release, with the sample size.

How to report the answer:
- Say whether the platform is **released** (announced by the station) or only a **prediction**. For a prediction, give the confidence the tool returns, and do not round it up.
- If a train shows no platform, say so. If the result says a platform was announced and later withdrawn, report only that it was announced earlier and has since been withdrawn. Never state, repeat or guess a withdrawn platform number.
- Give scheduled time, expected time and delay. If a train is cancelled, say so and give no platform.
- A train that is "due now" has reached its expected time with no departure recorded. Say that, and suggest checking the station screens.
- Mention how old the data is when the tool reports it as stale.
- If the result gives a time to check again (`next_check_s`), tell the user roughly when to ask again. Do not call the tools in a repeating loop.
- Always finish by saying predictions are unofficial and may be wrong, and the user should check the station screens before they move.

This connector is read only. It cannot book or change tickets, so if the user asks for that, say it is not something Platform Whisperer does and point them to a ticket retailer.
