# RideReady - Full-Stack Cab Booking & Dispatch System

RideReady is a full-stack cab booking application that models the workflow of booking, accepting, tracking, and completing rides. Built using the **MERN Stack** (MongoDB, Express.js, React.js, Node.js), RideReady offers dedicated dashboards for **Riders**, **Drivers**, and **Administrators** to simulate a live taxi dispatch system.

---

## 🚀 Key Features

### **Rider Portal**
*   **Authentication**: Register and log in securely.
*   **Book a Ride**: Input pickup and drop-off locations, schedule dates, choose available vehicles, and view **live fare estimates**.
*   **Real-Time Tracking**: Monitor dispatch states (Requested -> Driver Assigned -> Trip Started -> Arrived at Destination) with **automatic status polling**.
*   **Cashless Checkout & Rating**: Simulated credit checkout payment form and star rating/review feedback submission.
*   **Ride History**: Access previous ride logs and digital receipts.

### **Driver Portal**
*   **Vehicle Registration**: Register vehicle details (car name, category, seats, pricing, photo) directly during driver account creation.
*   **Console Status**: Toggle availability status (Online / Offline).
*   **Active Dispatch**: Fetch online pending requests, accept dispatch, start passenger trips, and complete dispatches.
*   **Earnings Tally**: Log concluded trips and track total income.

### **Admin Dashboard**
*   **Analytics Overview**: Platform statistics detailing total riders, drivers, vehicles, platform bookings, and total cashless revenue.
*   **User Management**: View, edit, and delete rider and driver profiles.
*   **Fleet Catalog (CRUD)**: Add new cabs, manage plate numbers, configure passenger capacities, adjust base rates, and upload vehicle photos.
*   **Supervision**: Overlook all platform bookings and manually adjust ride status.

---

## 🛠️ Technology Stack

*   **Frontend**: React.js (Vite), React Router v7, Axios, Bootstrap v5, Bootstrap Icons.
*   **Backend**: Node.js, Express.js.
*   **Database**: MongoDB, Mongoose ODM.
*   **Security & Auth**: JSON Web Tokens (JWT), BcryptJS.
*   **File Uploads**: Multer.

---

## 📂 Project Directory Structure

```
rideready/
├── server/                    # Node.js + Express.js Backend
│   ├── db/
│   │   └── config.js          # MongoDB Mongoose connection config
│   ├── models/
│   │   ├── User.js            # Rider & Driver schemas (availability & vehicle linkage)
│   │   ├── Admin.js           # Admin credentials
│   │   ├── Car.js             # Fleet vehicle specifications
│   │   ├── Ride.js            # Booking logs connecting Rider, Driver, and Car
│   │   ├── Payment.js         # Cashless credit transactions
│   │   └── Rating.js          # Feedback logs
│   ├── controllers/
│   │   ├── userController.js  # Registration, login, profiles for Riders & Drivers
│   │   ├── adminController.js # Admin authentication & user management
│   │   ├── carController.js   # Fleet CRUD management
│   │   └── rideController.js  # Dispatch logic: request, accept, start, complete, rate
│   ├── routes/
│   │   ├── userRoutes.js      # User auth & status toggles endpoints
│   │   ├── adminRoutes.js     # Admin user edit endpoints
│   │   ├── carRoutes.js       # Fleet CRUD endpoints
│   │   └── rideRoutes.js      # Dispatch workflow endpoints
│   ├── middlewares/
│   │   ├── authMiddleware.js  # JWT validation & role guard checks
│   │   └── multer.js          # Disk storage config for file uploads
│   ├── uploads/               # Car images uploaded via multer
│   ├── .env                   # Environment secrets (port, mongo URL, JWT secret)
│   ├── server.js              # Application entry point
│   └── verifyWorkflow.js      # E2E console test simulation script
│
└── client/                    # React.js Frontend (Vite scaffolded)
    ├── src/
    │   ├── components/
    │   │   ├── Rnav.jsx       # Rider navigation header
    │   │   ├── Dnav.jsx       # Driver navigation header
    │   │   └── Anav.jsx       # Admin navigation header
    │   ├── pages/
    │   │   ├── Home.jsx       # Public landing screen with Sarah's story
    │   │   ├── Login.jsx      # Rider/Driver Login
    │   │   ├── Register.jsx   # Rider/Driver Registration
    │   │   ├── Rhome.jsx      # Rider Dashboard (stats & quick actions)
    │   │   ├── RequestRide.jsx # Book cab with live fare estimation
    │   │   ├── RideTracking.jsx # Tracking, Checkout & Rating page
    │   │   ├── Rhistory.jsx   # Rider's ride history log
    │   │   ├── Dhome.jsx      # Driver Dashboard (Online/Offline, requests panel)
    │   │   ├── Alogin.jsx     # Admin Login
    │   │   ├── Aregister.jsx  # Admin Registration
    │   │   ├── Ahome.jsx      # Admin Dashboard with metrics
    │   │   ├── Users.jsx      # Admin: view and delete users
    │   │   ├── UserEdit.jsx   # Admin: modify user/driver profiles
    │   │   ├── Bookings.jsx   # Admin: oversee all rides
    │   │   ├── Acabs.jsx      # Admin: fleet vehicle registry table
    │   │   ├── Acabedit.jsx   # Admin: edit car info (image uploads)
    │   │   └── Addcar.jsx     # Admin: register new car
    │   ├── App.jsx            # Main app router with role protection guards
    │   ├── main.jsx           # Mounting logic importing Bootstrap & Stylesheets
    │   └── index.css          # Styling system (gradients, animations, glassmorphism)
    ├── package.json
    └── vite.config.js
```

---

## ⚙️ Setup & Configuration

### **Prerequisites**
Ensure you have **Node.js (v16+)** and **MongoDB** installed on your system.

### **1. Backend Server Setup**
1.  Navigate to the server directory:
    ```bash
    cd server
    ```
2.  Install dependencies:
    ```bash
    npm install
    ```
3.  Configure variables in the `.env` file:
    ```env
    PORT=8000
    MONGO_URI=mongodb://127.0.0.1:27017/ucab
    JWT_SECRET=ucab_secret_jwt_key_12345
    ```
4.  Run E2E database verification test script (optional):
    ```bash
    node verifyWorkflow.js
    ```
5.  Start the Express server:
    ```bash
    npm start
    ```

### **2. Frontend Client Setup**
1.  Open a new terminal tab/window and navigate to the client directory:
    ```bash
    cd client
    ```
2.  Install dependencies:
    ```bash
    npm install
    ```
3.  Start the Vite React development server:
    ```bash
    npm run dev
    ```
4.  Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## 📡 API Endpoints Documentation

### **User Endpoints (`/api/users`)**
*   `POST /register`: Create rider/driver account (supporting vehicle uploads). Body: `{ name, email, phone, password, role }`.
*   `POST /login`: Log in user. Body: `{ email, password }`.
*   `GET /profile`: Fetch active profile info. Headers: `Authorization: Bearer <token>`.
*   `PUT /availability`: Toggle driver availability online/offline. Headers: `Authorization: Bearer <token>`.

### **Admin Endpoints (`/api/admins`)**
*   `POST /register`: Create admin profile. Body: `{ username, email, password }`.
*   `POST /login`: Admin login. Body: `{ email, password }`.
*   `GET /users`: Get all users list. Headers: `Authorization: Bearer <adminToken>`.
*   `PUT /users/:id`: Edit user profile details. Headers: `Authorization: Bearer <adminToken>`.
*   `DELETE /users/:id`: Delete a user account. Headers: `Authorization: Bearer <adminToken>`.

### **Car Endpoints (`/api/cars`)**
*   `GET /`: Get all cars. Headers: `Authorization: Bearer <token>`.
*   `GET /:id`: Get specific cab details. Headers: `Authorization: Bearer <token>`.
*   `POST /`: Add a vehicle. Headers: `Authorization: Bearer <adminToken>` (Form-data: `name`, `model`, `plateNumber`, `seats`, `pricePerKm`, optional `image`).
*   `PUT /:id`: Update car specifications. Headers: `Authorization: Bearer <adminToken>`.
*   `DELETE /:id`: Delete car record. Headers: `Authorization: Bearer <adminToken>`.

### **Ride Endpoints (`/api/rides`)**
*   `POST /request`: Rider requests a ride. Headers: `Authorization: Bearer <token>`. Body: `{ carId, pickupLocation, dropLocation, bookingDate }`.
*   `GET /pending`: Get online pending dispatches (for drivers). Headers: `Authorization: Bearer <token>`.
*   `PUT /accept/:id`: Driver accepts dispatch. Headers: `Authorization: Bearer <token>`.
*   `PUT /start/:id`: Driver starts trip. Headers: `Authorization: Bearer <token>`.
*   `PUT /complete/:id`: Driver completes trip. Headers: `Authorization: Bearer <token>`.
*   `POST /pay`: Rider cashless card payment. Headers: `Authorization: Bearer <token>`. Body: `{ rideId, amount }`.
*   `POST /rate`: Rider leaves star feedback. Headers: `Authorization: Bearer <token>`. Body: `{ rideId, driverId, stars, comment }`.
*   `GET /rider/history`: Rider personal history. Headers: `Authorization: Bearer <token>`.
*   `GET /driver/history`: Driver completed trip history. Headers: `Authorization: Bearer <token>`.
*   `GET /admin/all`: Admin master logs. Headers: `Authorization: Bearer <adminToken>`.
*   `PUT /admin/status/:id`: Admin manual status override. Headers: `Authorization: Bearer <adminToken>`. Body: `{ status }`.

---

## 📌 Suggested Git Commit Messages

Use these descriptive commit messages when committing features to Git:
*   `feat(server): refactor models to support User, Admin, Car, Ride, Payment, and Rating`
*   `feat(controllers): implement driver acceptance dispatch matching, ratings, and checkout logs`
*   `feat(routes): map rider request, pending dispatch, starts, completes, payments, and ratings`
*   `feat(client): update App router guards for riders, drivers, and administrators`
*   `feat(pages): build online-offline toggles and real-time status polling for ride tracking`
*   `feat(testing): add E2E verification test verifyWorkflow simulating complete ride workflow`
*   `docs: update README installation guides and API endpoint lists`
