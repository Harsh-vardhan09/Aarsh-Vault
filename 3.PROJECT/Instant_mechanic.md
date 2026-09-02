> **An internal dashboard used by Instant Mechanic's operations team to monitor vehicle-service bookings, mechanics, customers and revenue.**


```
Booking status changes
        │
        ▼
    Backend
        │
        ├──────────────► PostgreSQL
        │
        └──────────────► WebSocket/SSE
                              │
                              ▼
                         Dashboard
                         updates
                         automatically
```


## Functional requirements
### Frontend

- Dashboard
- Bookings
- Mechanics
- Analytics
- Search
- Filters
- Sorting
- Pagination
- Loading states
- Error states
- Empty states
- Responsive UI

### Backend

- REST API
- Proper validation
- Error handling
- Booking APIs
- Dashboard statistics API
- Mechanics API
- Customers API



## HLD

![[_Excalidraw/Projects.md#^frame=IoXDR2wh9HqyLe0aEsGeu|100%]]

j7FsKad28KdLrxGL050IU4WRShgx2QoQ6iGvI/GkY6v9lxTal9Neh2OuV5sPkRmi