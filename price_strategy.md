### After reviewing this code, below is our current price strategy 
##### More organization-based rules applied for organization users and not listed here
- Viewers must have at least $20 before request streaming 

```javascript
GigService.request
PurchaseService.authorize
```
+ For happy path
Viewer will be refunded OR charged more at the end of the streaming session depends on the total they spent

```javascript
GigService.stop
PurchaseService.release
```
+ For branch cases: The session is stopped BEFORE streaming (stopped by users or our cleanup service)

++ If the session lasted more than 5 mins, cancellation fee applied (no matter who stops/cancels the stream)

```javascript
ActionService.cleanupGig (for gig cancelled/expired by our cleanup)
ActionService.getPrices (for gig cancelled/stopped by users)
ActionService.getCancellationPenalty
```
The ActionService.getCancellationPenalty is NOT used?

++ Otherwisewise
Price 0 applied
