# Run npm install to install node_modules and npm start to start the program. Program simulates train companies database using SQL to create database with a simple input form used to actively populate the database. Thorough explanation of idea and database requirements below. Was done in a group.

Cyclone Train Ticketing System

Sebastian Gajardo
Bryan Gronberg

Project URL on OSU servers: http://flip1.engr.oregonstate.edu:3234/

Project Outline

a) Overview

A small city is reinvesting in its low cost railway in an effort to appeal to tourists, commuters, and young people looking to relocate downtown. Cyclone trains are projected to generate $10 million in trip sales revenue every year. There are 15 unique trips which connect 5 stations and it is expected that 2,000 to 3,000 tickets will be sold each day across all stations in the city. Cyclone trains consist of one train type with a capacity for 200 passengers and because the system is low cost, a trip has one cost regardless of day or time.

In the past, the ticketing system was criticized for being outdated. There was no way for ticket collectors to validate or issue tickets electronically. Passengers needed to purchase their tickets at the station. It was also difficult for the city to analyze purchase activity because minimal data was collected at the point of sale. To support the improvements to the infrastructure of the railway, additional investment will be made to design a database backend for purchasing tickets. By creating this database, mobile and web applications can later be designed as front end portals for customers. Also, electronic ticket validation can happen consistently and in real time while keeping track of sales and passenger numbers.

Passengers can buy zero or many Tickets (1:M); and many Trips can be taken by many Passengers (M:M). A single ticket must belong to a single Passenger (1:1), and there’s one Trip per individual Ticket (1:1). Zero or more Tickets can be bought for one Trip (M:1) A Ticket must have a single Trip assigned to it (1:1) and can have zero or one Train assigned to it. Each Trip must have one Departure and one Arrival (1:1). A Train can be on zero or many Tickets (train can be NULL in case the train number is unknown at point of sale (regular train needs maintenance, was decommissioned etc.)). Arrival and departure stations can be part of many Trips or none (1:M) (NULL, can add arrival and departure stations without connecting to Trip). 



b) Database Outline, in Words

Concept: Train Ticket System

Entities: 

Passengers (Entity): Record details of passengers that ride our trains.
passengerId: INT, auto_increment, unique, not NULL, PK.
fistName: TEXT, NULLable.
lastName: TEXT, NULLable.
dob: DATE, NULLable.
relationships: 1:M with Tickets with passengerID as an FK inside Tickets.

Trips (Entity): Available trips on our train system and prices associated with them. Each departure and arrival pairing is given a tripId number to act as the PK for simplicity.
tripId: INT, auto_increment, unique, not NULL, PK.
departureId: INT, not Null, FK.
arrivalId: INT, not Null, FK.
price: DECIMAL, not NULL.
relationships: 1:M with Tickets with tripId as an FK inside Tickets.

Tickets (Entity & Intersection Table): Keep track of tickets sold per date. Also acts as an intersection table between Passengers and Trips. TrainId is optional and can be NULLED. 
ticketNumber: INT, auto_increment, unique, not NULL, PK.
passengerId: INT, not NULL, FK. 
tripId: INT, not NULL, FK.
trainId: INT, NULLABLE, FK.
tripDate: DATE, not NULL.
relationships: Intersection table for Passengers and Trips M:M relationship, M:1 for both with passengerID and tripID as FK’s. Also M:1 with Trains with trainId as FK.

Departures (Entity): Stations trains depart at.
departureId: CHAR, unique, not NULL, PK.
departureLocation: VARCHAR, not NULL.
relationships: 1:M with Trips, with departureId as an FK inside Trips.

Arrivals (Entity): Stations trains arrive at.
arrivalId: CHAR, unique, not NULL, PK.
arrivalLocation: VARCHAR, not NULL.
relationships: 1:M with Trips, with arrivalId as an FK inside Trips.

Trains (Entity): Details of trains owned by the company, including if in operation or maintenance. All trains have a capacity of 200. 
trainId: INT, auto_increment, unique, not NULL, PK.
capacity: INT, not NULL.
inOperation: BOOL, not NULL.
relationships: 1:M with Tickets with trainId as a NULLABLE FK in Tickets.

