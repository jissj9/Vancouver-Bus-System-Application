
## TODO List

### Project prerequisites

#### Setup group
- [x] Find 4 members
- [x] Organise communication
- [x] Discuss project plan
- [x] Divvy up work

#### Setup project repository
- [x] Initialize git repository on GitHub
- [x] Invite group members
- [x] Invite duspari
- [x] Project structure template

### Project features

#### Shortest paths between 2 bus stops
- [x] Pre-load input files
- [x] Parse stop id to calculate cost
- [x] Create graph from nodes and edge cost
- [x] Search shortest path on created graph
- [x] Enable user input
- [x] Error checking
  - [x] Invalid bus stop_id
  - [x] No possible routes
  - [x] Same user input for start and destination stops 

#### Searching by bus stop names using a TST
- [x] Pre-process bus stops
  - [x] Move keywords to end of name string
  - [x] Add bus stop names to TST
  - [x] Map bus stop names to ids
- [x] Move keywords to end of string
- [x] Look for search term in TST
- [x] Find all of the stops that have search term as prefix
- [x] Get bus stop info from the found stops
- [ ] Edge cases
  - [x] Empty search query
  - [x] No stops
  - [x] Search query not in stops
  - [x] Search query is exact same as stop name

#### Searching by trip arrival times
- [x] Agree on methods signature(s) with Varjak
- [x] Pre-loading / Pre-processing input files
- [x] How to quickly search `stop_times.txt` by `hh:mm:ss`?
- [x] How to quickly sort by `trip id`?
- [x] Sanitised inputs
  - [x] Prune invalid times
  - [x] Maximum time

#### Application interface
- [x] Interfaces
  - [x] Search Shortest Paths
  - [x] Search Arrival Times
  - [x] Search Stops
- [x] Error Handling
  - [x] Invalid file inputs
  - [x] Time Format For Arrival Times
  - [x] No Results For Stop Search

#### Additional files
- [x] Add template files for all text deliverables ([TODO list](./TODO.md), [Design Document](./DESIGN_DOCUMENT.md))
- [x] Record a demo of our project (maximum 5mins)
- [x] Write up our design decisions in the [Design Document](./DESIGN_DOCUMENT.md)
- [x] Remove the notes at the top of the supplementary files

