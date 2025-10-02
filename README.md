
# The "Tiered Ticket Sales" Challenge

We'll build upon the Event Ticketing Service scenario by introducing tiered tickets and a multi-step reservation process.

## Scenario

The Event Ticketing Service now needs to manage inventory across three tiers: **VIP**, **General Admission (GA)**, and **Student**. Each event has a fixed total capacity, but the number of tickets allocated to each tier can be adjusted dynamically (e.g., if GA sells out, we might reallocate some Student tickets to GA, up to the total event capacity).

The reservation process is also now two-phase:

- Phase 1: Hold (Pre-Payment): A user selects a ticket tier and quantity. The system must hold those tickets for 10 minutes, reducing the available stock for that tier.

- Phase 2: Confirmation/Cancellation: The user either confirms (payment successful, tickets sold) or the hold expires (tickets released back to the stock).

The system must handle concurrent attempts to hold tickets across different tiers for the same event.

## The assignment

Implement a thread-safe class, **TieredStockManager**, which is initialized with the initial stock for VIP and GA tiers.

- Implement a method: _tryHold(tierName: String, quantity: Int): Boolean_

This method must atomically check if enough stock is available for the given tier. If available, it reduces the Available Stock for that tier and returns a transaction id. It must ensure that holding tickets in one tier (e.g., VIP) does not block another thread trying to hold tickets in another tier (e.g., GA).

- Implement a companion method: _commitHold(tierName: String, quantity: Int): Boolean_

This simulates a successful purchase. It decrements the held amount from the final Total Sold count (implicitly, you can just treat the current stock as 'on hold').

- Implement a method: _releaseHold(tierName: String, quantity: Int)_

This simulates an expired/cancelled reservation and returns the tickets back to the available stock for that tier.

The class should maintain the total available stock for each tier, and these operations must be thread-safe. If GA tickets are sold out, but there are still Student tickets available, the system should allow holding Student tickets without blocking.

