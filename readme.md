# Project Title

## Overview

AlgoRace - A multiplayer data structure and algorithm problem solving platform.
Link to [site](algorace-frontend.vercel.app)

### Problem

Leetcode is good but after a while it starts to get monotonous not being able to collaborate or do it alongside friends. AlgoRace lets users
solve data structures and algorthims on their own, or against friends.

### User Profile

The target audience is younger software engineers, those either learning to solve DSA's, preparing for job interviews, or those who just enjoy them

### Features

AlgoRace boasts a completely custom compiler utilizing a containerized and distributed architecture allowing us to safely compile and test user code. 

Users can either solve problems of their choosing in 'Solo/Practice' mode or in a lobby setting with 'Challenge' mode. 

When in either practice or challenge mode, the user can select their preferred language, although defaulted to JavaScript.

## Implementation

### Tech Stack

AlgoRace, built using a microservice architecture is built with Node, Docker, RabbitMQ, Bash, SocketIO, MongoDB and React.
The services are hosted on Heroku, the worker service is hosted on an AWS EC2 instance for operating system level access.

The system has been designed with high availability, security, and speed in mind. AlgoRace utilizies 3 services total. 

- User Service: Handles all authentication and displaying of data
- Socket Server: Handles connection and logic for multiplayer mode, connecting users to their rooms.
- Manager Service: Is the first reciever of a request to compile code. This creates a CompileJob, places the job into the task queue, and returns the
jobId to the client for polling
- Worker Service: The worker is subscribed to the task queue and is detached from the manager, only communicating through the queue. We did this
so as concurrent requests to compile grow, we can horizontally scale easily since we don't have any direct links from Manager - Worker. The worker spins up
a Docker container with the users code and everything else needed, which runs a Bash script that runs the users code. Upon completion, the Worker sends a request
to the manager with the stderr or stdout and jobId.
The Worker Service is capable of multiple languages and the supported language list is continuously growing in size as well as total problems.

### APIs

Endpoints and service-level documentation can be found in their respective repos linked as a sub-module in this repository.

### Sitemap

Login - User can login or click to create an account
Sign up - Linked on Login, users can create an account
Home - Users can start a Challenge lobby, join one, or start up a practice session
Practice - Users can select a problem and complete it in our editor
Challenge Lobby - Users can ready/unready themselves, when all ready, the host can start the match
Challenge - Users in lobby are all given the same problem to solve each round


### Mockups

Compile system architecture:
![System design and architecture of compilation](/assets/systemdesign.png "System Design Architecture")

### Data

Data is stored using MongoDB. In terms of the problems, there is a collection 'problems' which is a master list with the information about problem titles, their difficulty, and type.
Problem code (starter code, and meta data about the problem which contains code for compiler to use to carry out compilation) stores the indepth info for a problem which is unique by the title and language together 
(similar to a composite key in a SQL database)

### Auth

User authentication is carried out using JWT, stored in the client with cookies.

## Roadmap

Next steps will be to continue adding problems for users to solve and expanding the available languages for each problem. Upon completion of this there will be a public API built for developers to compile code remotely.


