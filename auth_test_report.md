# Authorization Test Report - Phase 3

##  GUEST (Not logged in)
###  Can do:
- View public booking list — http://localhost:8004/
- View login page — http://localhost:8004/login
- View registration page — http://localhost:8004/register
- View privacy policy — /privacy
- View terms of service — /terms
- View cookie policy — /cookies

###  Cannot do:
- Access dashboard — http://localhost:8004/dashboard (redirects to login)
- Add new resource — http://localhost:8004/resources/new (redirects to login)
- Add new reservation — http://localhost:8004/reservations/new (redirects to login)
- Access any user-specific pages
- Log out (no session)
- View any user profiles
- Create resources or reservations

##  RESERVER (Logged in as reserver1@test.com)
###  Can do:
- Access personal dashboard — http://localhost:8004/dashboard
- Add new resource — http://localhost:8004/resources/new
- Add new reservation — http://localhost:8004/reservations/new
- View booked resources — http://localhost:8004/
- Log out — http://localhost:8004/logout
- Edit and delete OWN reservations
- Create resources (Meeting Room, Projector, etc.)

###  Cannot do:
- Access /admin — 404 Not Found
- Access /admin/users — 404 Not Found
- Access /users — 404 Not Found
- Access /manage — 404 Not Found
- Access /administrator — 404 Not Found
- **Cannot edit or delete reservations made by admin** — tested with reservation "lkn" created by admin@test.com
- **Cannot edit or delete reservations made by other reservers** — tested with reservation "qwe" created by reserver1@test.com (cannot modify as reserver2)
- Delete other users' reservations
- Modify resources created by others
- View other users' personal information
- Delete users
- Change other users' roles

##  ADMINISTRATOR (Logged in as admin@test.com)
###  Can do:
- Access dashboard — http://localhost:8004/dashboard
- Add new resource — http://localhost:8004/resources/new
- Add new reservation — http://localhost:8004/reservations/new
- View all booked resources — http://localhost:8004/
- Log out — http://localhost:8004/logout
- **Edit and delete ALL reservations** — both own reservations and those made by reservers
- Full control over all resources and reservations
- Can modify "lkn" (own reservation) and "qwe" (reserver1's reservation)

###  Cannot do:
- Access /admin — 404 Not Found (no separate admin panel)
- Access /admin/users — 404 Not Found
- Access /users — 404 Not Found
- Access /manage — 404 Not Found
- Access /administrator — 404 Not Found
- No visible user management interface (cannot delete users directly from UI)

##  Summary of Findings
-  **Proper authorization:** Reserver can only modify own reservations
-  **Admin privileges:** Admin can modify all reservations
-  **Access control:** No role can access non-existent admin pages
- ️ **Missing feature:** No user management interface for admin
-  **Matches spec:** Reservers cannot access admin functions
