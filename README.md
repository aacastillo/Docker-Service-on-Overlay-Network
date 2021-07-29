# Docker-Service-on-Overlay-Network
### Description
We are gonna build three services for a hypothetical fruit stand company. We are going to create a software that calculates and serves a list of products and their prices, taking into account whether they are on sale.

The software consists of three components:
- A ```base-price``` component which serves the base prices of fruits sold by the fruit stand
- a `sales` component which provides a list of items that are on sale and how much each item is discounted
- a `total-price` component which communicates with the other two and calculates the final price for each item.

We are going to use a swarm cluster and run these application components as services in the swarm cluster. We will run them on a custom overlay network to facilitate isolated communication between them.

### Prerequisites
- We will need to initialize the docker swarm manager and two worker nodes
- Install curl onto the swarm manager

### Dependencies
We will be using 3 different public linuxacademy images for the three different components.
- linuxacademycontent/prices-base-price:1
- linuxacademycontent/prices-sales:1
- linuxacademycontent/prices-total-price:1

### Installation
1. ssh into the swarm manager
2. Create the `prices-overlay-net` overlay network
```
docker network create --driver overlay prices-overlay-net
```

3. Create the `base-price` service

```
docker service create --name base-price --network prices-overlay-net --replicas 3 linuxacademycontent/prices-base-price:1
```
4. Create the `sales` service

```
docker service create --name sales --network prices-overlay-net --replicas 3 linuxacademycontent/prices-sales:1
```
5. Create the `total-price` service

```
docker service create --name total-price --network prices-overlay-net --replicas 2 -p 8080:80 linuxacademycontent/prices-total-price:1
```
6. Verify that you can the total price data:

`curl localhost:8080`
