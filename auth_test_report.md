# Authorization Test Report - Phase 3

## 👤 GUEST (Not logged in)
### ✅ Can do:
- View public booking list — http://localhost:8004/
- View login page — http://localhost:8004/login
- View registration page — http://localhost:8004/register
- View resources via API — GET /api/resources
- View reservations via API — GET /api/reservations

### ❌ Cannot do:
- Access /resources — redirects to login
- Access /reservations/new — redirects to login
- Create new resources — POST /api/resources (401 Unauthorized)
- Create new reservations — POST /api/reservations (401 Unauthorized)
- Edit or delete any reservations
- Access any authenticated pages

## 👤 RESERVER (Logged in as reserver1@test.com)
### ✅ Can do:
- View home/booking list — http://localhost:8004/
- Add new resource — http://localhost:8004/resources/new
- Add new reservation — http://localhost:8004/reservations/new
- Create resources via API — POST /api/resources
- Create reservations via API — POST /api/reservations
- Edit and delete OWN reservations
- View all resources and reservations
- Log out — http://localhost:8004/logout

### ❌ Cannot do:
- Access /admin — 404 Not Found
- Access /admin/users — 404 Not Found
- Access /users — 404 Not Found
- Access /manage — 404 Not Found
- Cannot edit or delete reservations made by admin
- Cannot edit or delete reservations made by other reservers
- Cannot delete other users' reservations
- Cannot modify resources created by others
- Cannot view other users' personal information
- Cannot delete users
- Cannot change other users' roles

## 👑 ADMINISTRATOR (Logged in as admin@test.com)
### ✅ Can do:
- View home/booking list — http://localhost:8004/
- Add new resource — http://localhost:8004/resources/new
- Add new reservation — http://localhost:8004/reservations/new
- View all booked resources — http://localhost:8004/
- Create resources via API — POST /api/resources
- Create reservations via API — POST /api/reservations
- **Edit and delete ALL reservations** — both own and others'
- Full control over all resources and reservations
- Log out — http://localhost:8004/logout

### ❌ Cannot do:
- Access /admin — 404 Not Found (no separate admin panel)
- Access /admin/users — 404 Not Found
- Access /users — 404 Not Found
- Access /manage — 404 Not Found
- No visible user management interface (cannot delete users directly from UI)

## 🔌 API Endpoints Testing

### ✅ Discovered API Endpoints
| Endpoint | Method | Response Type | Access |
|----------|--------|---------------|--------|
| /api/resources | GET | JSON | Public (Guest) |
| /api/reservations | GET | JSON | Public (Guest) |
| /resources | GET/POST | HTML | Authenticated |
| /reservations/new | GET/POST | HTML | Authenticated |

### 🔐 API Authorization Testing

**Public Endpoints (Guest Access):**
- `GET /api/resources` → Returns list of resources
- `GET /api/reservations` → Returns reservations without reserver identity

**Authenticated Endpoints (Requires Login):**
- `POST /api/resources` → Requires authentication
- `POST /api/reservations` → Requires authentication
- `DELETE /api/reservations/:id` → Only own reservations (reserver) or any (admin)
- `PUT /api/reservations/:id` → Only own reservations (reserver) or any (admin)

### 📊 Authorization Matrix

| Action | Endpoint | Guest | Reserver | Admin |
|--------|----------|-------|----------|-------|
| View resources | GET /api/resources | ✅ | ✅ | ✅ |
| View reservations | GET /api/reservations | ✅ | ✅ | ✅ |
| Create resource | POST /api/resources | ❌ | ✅ | ✅ |
| Create reservation | POST /api/reservations | ❌ | ✅ | ✅ |
| Edit own reservation | PUT /api/reservations/:id | ❌ | ✅ | ✅ |
| Edit other's reservation | PUT /api/reservations/:id | ❌ | ❌ | ✅ |
| Delete own reservation | DELETE /api/reservations/:id | ❌ | ✅ | ✅ |
| Delete other's reservation | DELETE /api/reservations/:id | ❌ | ❌ | ✅ |

### 🔒 Security Summary
- ✅ Proper authorization: Reserver can only modify own reservations
- ✅ Admin privileges: Admin can modify all reservations
- ✅ Guest restrictions: Guest cannot POST/PUT/DELETE any API endpoints
- ✅ No IDOR vulnerability: Attempting to access other users' reservation IDs returns 403
- ✅ API matches spec: Guest can view bookings without reserver identity
- ⚠️ Missing: No admin-specific API endpoints for user management
