# Design Airline Check-in System

- Multiple Airlines
- Every airline has multiple flights
- Each flight has 120 Seats
- Every flight has multiple trips
- User books a seat in one trip of a flight
- **Agenda:** Handle multiple people trying to pick seats on the plane

### Schema

![Screenshot 2023-08-19 at 3.57.12 PM.png](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20Airline%20Check-in%20System/airline-checkin-system-schema.png)

### High level Architecture

![Screenshot 2023-08-19 at 3.58.47 PM.png](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20Airline%20Check-in%20System/airline-checkin-system-hla.png)

### So how we make our transaction efficient with having one user take one seat

**FIXED INVENTORY + CONTENSION = LOCKING**

We have 120 Seats on our plane [FIXED INVENTORY]

Multiple Users trying to book same seats [CONTENSION]

We use locking to prevent multiple users having same seat booked [LOCKING]
