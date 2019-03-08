

## SYSTEM REQUIREMENTS

- Docker 18.09.1
- Wercker 1.0.1467
-

## DOCKER REQUIREMENTS

### DEV

- 4 CPU's
- 3 GB RAM
- 1.5 GB SWAP

Startup time: 11 min.

try:
- 8 CPU's
- 8 GB RAM
- 10 GB SWAP

start: 19:29:30
end: 19:41:00
startup time: 10.5 mins
notes: still seems slow , will try tomorrow

### MINIMUM LIVE REQUIREMENTS

$5 DO instance:
- 1 CPU
- 1 GB RAM
- 4 GB SWAP

1:
start: 18:05:00
end: 18:20:00
startup time: 15 min.
notes: requests are processed very slowly, may need to get upgraded package.

2 (with --production flag):
start: 18:27:15
end: 19:06:00
startup time: 40 min.
notes: long to start in dev mode. would have series issues passing wercker checks.
may have to abandon wercker set up if this is true.

$10 DO instance:
- 1 CPU
- 2 GB RAM
- 4 GB SWAP

1:
start:
end:
startup time:
notes:

2 (with --production flag):
start:
end:
startup time:
notes:

*********************************

  IMPORTANT! DEFAULT ADMIN INFO

  EMAIL/LOGIN: admin@localhost

  PASSWORD: r3@cti0n

*********************************


## Deploy to dokku

watch out for checks, will need to make these long in order to ensure long initial setup does not time out.
