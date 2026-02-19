# Smart-Parking-Lot-System
🚗 Smart Parking Lot Backend System
📌 Project Overview

This project implements the Low-Level Design (LLD) of a Smart Parking Lot backend system.
The system manages vehicle entry and exit, automatic parking spot allocation, real-time availability tracking, and parking fee calculation.

It is designed to handle concurrency efficiently and can scale to support multi-floor parking lots in urban environments.

🎯 Objective

Design a backend system that:

Automatically assigns parking spots based on vehicle size

Records entry and exit times

Calculates parking fees

Updates parking availability in real-time

Handles concurrent vehicle operations safely

🏗 System Architecture

The backend system consists of the following logical services:

Entry Service – Handles vehicle check-in

Exit Service – Handles vehicle check-out

Spot Allocation Service – Assigns parking spots

Pricing Service – Calculates parking fees

Availability Service – Maintains real-time spot status

Transaction Service – Stores parking history

Concurrency Manager – Prevents double booking

🚙 Functional Requirements
1️⃣ Parking Spot Allocation

Assign available spots automatically

Based on vehicle type:

MOTORCYCLE

CAR

BUS

Nearest floor preferred

2️⃣ Check-In

Record vehicle entry time

Assign parking spot

Create parking ticket

3️⃣ Check-Out

Record exit time

Calculate fee

Mark spot as available

4️⃣ Real-Time Updates

Update spot availability immediately

Support display boards or dashboards

🗄 Database Schema
Vehicle
Field	Type
vehicle_id (PK)	UUID
vehicle_number	String
vehicle_type	ENUM
ParkingFloor
Field	Type
floor_id (PK)	UUID
floor_number	Integer
ParkingSpot
Field	Type
spot_id (PK)	UUID
floor_id (FK)	UUID
spot_type	ENUM
is_available	Boolean
ParkingTicket
Field	Type
ticket_id (PK)	UUID
vehicle_id (FK)	UUID
spot_id (FK)	UUID
entry_time	Timestamp
exit_time	Timestamp
total_fee	Decimal
status	ENUM (ACTIVE, COMPLETED)
PricingConfig
Field	Type
vehicle_type	ENUM
price_per_hour	Decimal
🧠 Spot Allocation Algorithm
1. Identify vehicle type
2. Fetch eligible available spots
3. Sort by:
   - Floor number (ascending)
   - Spot ID
4. Lock the selected spot
5. Mark as unavailable
6. Return assigned spot

Optimization

Maintain in-memory structure:

availableSpots[vehicleType][floor] → MinHeap

💰 Parking Fee Calculation
Formula
Total Fee = ceil(duration_in_hours) × price_per_hour

Example
Vehicle	Duration	Rate	Fee
Car	2.5 hrs	₹50/hr	₹150
Bike	1.2 hrs	₹20/hr	₹40
🔐 Concurrency Handling

To prevent multiple vehicles from being assigned the same spot:

Option 1 – Database Locking
SELECT ... FOR UPDATE

Option 2 – Optimistic Locking

Add version column to ParkingSpot

Option 3 – Distributed Lock (Recommended)

Redis-based locking

Lock key: spot:{spot_id}

🔄 Real-Time Availability

Updates triggered on:

Vehicle entry

Vehicle exit

Possible implementation:

WebSockets

Kafka events

Redis Pub/Sub

📡 Sample API Endpoints
Vehicle Entry
POST /api/parking/entry


Request Body:

{
  "vehicleNumber": "KA01AB1234",
  "vehicleType": "CAR"
}

Vehicle Exit
POST /api/parking/exit


Request Body:

{
  "ticketId": "T123"
}

🚀 Scalability Considerations

Horizontal scaling using stateless services

Database indexing on is_available

Caching available spots in Redis

Event-driven updates for high throughput

📈 Future Enhancements

Reservation system

Dynamic pricing (peak hours)

License plate recognition

Multi-location support

Admin analytics dashboard

Mobile app integration

🧪 Edge Cases Handled

No available spots

Duplicate vehicle entry

Exit without active ticket

Simultaneous spot allocation

Partial hour billing

📚 Tech Stack (Example)

Backend: Java / Spring Boot or Python / FastAPI

Database: PostgreSQL / MySQL

Cache: Redis

Messaging: Kafka (Optional)

Deployment: Docker + Kubernetes

🏁 Conclusion

This Smart Parking Lot backend system provides:

Efficient parking spot management

Accurate billing

Real-time availability updates

Safe concurrent operations

Scalable architecture
