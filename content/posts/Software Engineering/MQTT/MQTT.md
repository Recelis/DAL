---
title: "MQTT"
date: 2024-04-22T22:25:06.653Z
---

# MQTT

[docs](https://www.hivemq.com/blog/how-to-get-started-with-mqtt/#heading-mqtt-tutorial-an-easy-guide-to-getting-started-with-mqtt)
[docs](http://www.steves-internet-guide.com/mqtt/)

MQTT (Message Queuing Telemetry Transport) is a `publish/subscribe` messaging protocol. This is basically like having TV broadcaster broadcasting a TV show on a channel and having multiple viewers tuning in to watch that TV show.

The airways here is the `MQTT Broker` who broadcasts the TV show on many channels (in this case called `topics`). The broadcaster and the viewers are the MQTT Clients who can both subscribe to and publish content.

One thing that a MQTT broker can do is to `filter messages` by `topic` and distribute this to `subscriber`.

## MQTT Clients

MQTT Cliets connect to the MQTT Broker via TCP/IP which establishes a connection before a message can be published. They also publish a `keepaive message` at regular intervals for the MQTT Broker to know that the client is still connected.

## MQTT Broker

Also known now as MQTT Servers.

## Persistent Sessions

By default, MQTT Client will set up `clean sessions` with brokers, but you can have non-clean sessions aka persistent sessions that will remember client subscriptions and undelivered messages aka `retained messages`. This will depend on the `Quality of service` when subscribing and publishing to topics.

## Last Will and Testament (LWT)

A LWT message is the notification of the subscriber if a publisher is unavailable. THis is set up per topic by each publishing client. When a broker doesn't receive the `keepalive message` within the expected interval, then the broker will send out the LWT message for that publishing client.
