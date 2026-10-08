# 🛒 Sharma Kirana Store

A modern, responsive grocery ordering web application built for **Sharma Kirana Store**.

The application provides a complete digital ordering experience where customers can browse grocery products, search and filter items, manage a shopping cart, sign in using Google, provide their delivery details and location, place orders through Firebase Firestore, and receive order communication through WhatsApp.

The project is designed as a lightweight **serverless web application**, using Firebase for authentication and cloud data storage while being deployable as a static website through GitHub Pages.

---

## ✨ Features

### 👤 Customer Features

* 🔐 Google Sign-In authentication
* 🛍️ Browse available grocery products
* 🔎 Product search
* 🗂️ Category-based product filtering
* 🛒 Add products to cart
* ➕ Increase/decrease product quantities
* 🗑️ Remove products from cart
* 💰 Automatic cart total calculation
* 📱 Mobile number collection
* 🏠 Delivery address collection
* 📍 Current-location detection using browser Geolocation API
* 📝 Delivery/order notes
* 📦 Order placement
* 🔢 Automatic order number generation
* 📲 WhatsApp order notification
* 📱 Responsive mobile-friendly interface
* 🖥️ Desktop browser support

---

## 🧑‍💼 Admin Features

The application includes an admin interface for managing and viewing incoming orders.

Admin functionality includes:

* 🔐 Admin authentication
* 📦 View customer orders
* 👤 View customer information
* 📞 View customer phone number
* 📧 View customer email
* 🏠 View delivery address
* 📍 Access customer location information
* 🛒 View ordered products
* 💰 View order total
* 📊 View order status
* 🔗 Access phone numbers through `tel:` links
* 📲 Order communication through WhatsApp

---

# 🏗️ System Architecture

The application follows a lightweight **serverless client-side architecture**.

```text
                         ┌─────────────────────────┐
                         │       Customer           │
                         │  Mobile / Desktop Browser│
                         └────────────┬────────────┘
                                      │
                                      │ HTTPS
                                      ▼
                         ┌─────────────────────────┐
                         │      GitHub Pages       │
                         │       Static Hosting     │
                         │                         │
                         │       index.html        │
                         └────────────┬────────────┘
                                      │
                  ┌───────────────────┼───────────────────┐
                  │                   │                   │
                  ▼                   ▼                   ▼
        ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
        │ Firebase Auth   │  │   Firestore     │  │ Browser APIs    │
        │                 │  │                 │  │                 │
        │ Google Sign-In  │  │ Products        │  │ Geolocation     │
        │ User Sessions   │  │ Users           │  │                 │
        └─────────────────┘  │ Orders          │  └─────────────────┘
                             └────────┬────────┘
                                      │
                                      ▼
                             ┌─────────────────┐
                             │ WhatsApp        │
                             │ Order Sharing    │
                             └─────────────────┘
```

---

# 🔄 Application Flow

```text
Customer
   │
   ▼
Open Website
   │
   ▼
Google Sign-In
   │
   ▼
Firebase Authentication
   │
   ▼
Browse Products
   │
   ▼
Search / Filter
   │
   ▼
Add Products to Cart
   │
   ▼
Checkout
   │
   ├── Mobile Number
   ├── Delivery Address
   ├── Current Location
   └── Order Note
   │
   ▼
Place Order
   │
   ▼
Validate Order
   │
   ▼
Create Firestore Order
   │
   ├───────────────┐
   │               │
   ▼               ▼
Firestore       WhatsApp
   │               │
   ▼               ▼
Admin Panel     Store Contact
```

---

# 🧰 Technology Stack

## Frontend

### HTML5

The application is implemented as a single-page HTML application.

HTML is responsible for:

* Page structure
* Product sections
* Navigation
* Cart interface
* Checkout form
* Admin interface
* Buttons and interactive elements

---

### CSS3

CSS is used for:

* Responsive layouts
* Product cards
* Navigation
* Cart UI
* Checkout UI
* Admin dashboard
* Buttons
* Forms
* Mobile responsiveness
* Visual styling

The application is designed to work across:

* Desktop
* Laptop
* Tablet
* Mobile browsers

---

### JavaScript

The application uses vanilla JavaScript for the main application logic.

JavaScript handles:

* Application state
* Product rendering
* Searching
* Category filtering
* Cart management
* Authentication
* Checkout validation
* Order creation
* Geolocation
* WhatsApp integration
* Admin functionality
* Firestore communication
* UI rendering

No frontend framework such as React, Angular or Vue is required.

---

# ☁️ Firebase

Firebase acts as the backend infrastructure for the application.

The project currently uses Firebase's browser-compatible SDK.

```text
Firebase
   │
   ├── Authentication
   │
   └── Cloud Firestore
```

---

## 🔐 Firebase Authentication

Google authentication is implemented using Firebase Authentication.

The application uses:

```javascript
firebase.auth()
```

and:

```javascript
firebase.auth.GoogleAuthProvider()
```

Customers can sign in using their Google account.

The authentication flow is:

```text
Customer
   │
   ▼
Google Sign-In
   │
   ▼
Firebase Authentication
   │
   ▼
Authenticated Firebase User
   │
   ▼
Application
```

The application listens for authentication changes using Firebase's authentication state listener.

---

# 🗄️ Cloud Firestore

Cloud Firestore is the application's cloud database.

The application uses Firestore for:

### Products

Products are loaded dynamically from Firestore.

The application listens for product changes using a Firestore realtime listener.

This allows the product catalogue to be updated without modifying the frontend code.

---

### Users

Customer profile information can be stored under:

```text
users/{uid}
```

The user document is associated with the authenticated Firebase user's UID.

The application can store information such as:

```text
phone
address
location
```

---

### Orders

Customer orders are stored in the:

```text
orders
```

collection.

An order contains information such as:

```text
uid
name
email
phone
address
location
note
items
total
status
order number
created timestamp
```

Conceptually:

```text
orders
   │
   ├── Order 1
   │     ├── uid
   │     ├── customer
   │     ├── phone
   │     ├── address
   │     ├── location
   │     ├── items
   │     ├── total
   │     ├── status
   │     └── created
   │
   ├── Order 2
   │
   └── Order 3
```

---

# 📍 Location Services

The application uses the browser's native Geolocation API.

```javascript
navigator.geolocation.getCurrentPosition()
```

When the customer chooses to share their location, the application obtains:

```text
Latitude
Longitude
```

The coordinates are rounded before being stored.

The location can then be used to construct a Google Maps location link.

Conceptually:

```text
Browser
   │
   ▼
Geolocation API
   │
   ▼
Latitude + Longitude
   │
   ▼
Order
   │
   ▼
Google Maps Link
```

Because browser geolocation requires a secure context, hosting the application through **HTTPS** is important.

---

# 📲 WhatsApp Integration

The application integrates WhatsApp using a WhatsApp `wa.me` URL.

After an order is placed, order information can be prepared for WhatsApp communication.

The message can include information such as:

* Customer name
* Phone number
* Delivery address
* Location
* Ordered products
* Total amount
* Customer note
* Order number

Conceptually:

```text
Order
  │
  ▼
Generate WhatsApp Message
  │
  ▼
wa.me
  │
  ▼
Store WhatsApp Number
```

This provides a simple communication channel between the customer and the store.

---

# 🛒 Shopping Cart Architecture

The cart is maintained on the client side.

The application maintains cart state in JavaScript:

```javascript
let cart = ...
```

Cart operations include:

```text
Add Product
     │
     ▼
Update Quantity
     │
     ▼
Calculate Total
     │
     ▼
Checkout
```

Each cart item contains product-related information required to calculate the order.

---

# 💾 Browser Storage

The current application uses browser storage for convenience.

### Local Storage

The application currently uses:

```javascript
localStorage
```

for:

```text
cart
profile
```

The helper functions are:

```javascript
jget()
jset()
```

This allows cart/profile information to persist between page loads in the same browser.

### Session Storage

The admin login state currently uses:

```javascript
sessionStorage
```

for the admin session flag.

The application uses:

```text
adm
```

to represent the active admin session.

### Firebase Authentication

Firebase Authentication also manages its own authentication session persistence internally.

---

# 🔐 Data Flow

## Customer Authentication

```text
Browser
   │
   ▼
Google Login
   │
   ▼
Firebase Authentication
   │
   ▼
Firebase UID
   │
   ▼
Application User Session
```

---

## Product Loading

```text
Firestore
   │
   ▼
Products Collection
   │
   ▼
Realtime Listener
   │
   ▼
JavaScript Product State
   │
   ▼
Product UI
```

---

## Order Placement

```text
Customer
   │
   ├── Products
   ├── Phone
   ├── Address
   ├── Location
   └── Note
   │
   ▼
Client-side Validation
   │
   ▼
Create Order Object
   │
   ▼
Firestore
   │
   ├── orders/{orderId}
   │
   └── users/{uid}
   │
   ▼
WhatsApp Notification
```

---

# 🧩 Application Components

Although the project is contained in a single HTML file, its JavaScript logic can be viewed as several logical modules.

```text
Application
│
├── Configuration
│
├── Firebase Initialization
│
├── Authentication
│
├── Product Management
│
├── Search & Filtering
│
├── Cart Management
│
├── Checkout
│
├── Geolocation
│
├── Order Management
│
├── WhatsApp Integration
│
├── Admin Authentication
│
└── Admin Order Management
```

---

# 🔎 Product Management

Products are retrieved from Firestore rather than being permanently hardcoded into the UI.

The application maintains product state in memory and renders products dynamically.

Supported operations include:

* Product loading
* Product display
* Product search
* Category filtering
* Product selection
* Add to cart

---

# 🔍 Search and Filtering

Customers can find products using:

### Search

Products can be searched based on product information.

### Categories

Products can be filtered by category.

This makes the application more suitable for a larger grocery catalogue.

---

# 🧾 Checkout Validation

Before an order is created, the application validates required customer information.

The phone number is validated against an Indian mobile number format:

```text
6XXXXXXXXX
7XXXXXXXXX
8XXXXXXXXX
9XXXXXXXXX
```

The application also validates the delivery address.

Orders are only submitted after the required information passes validation.

---

# 📦 Order Number

Every order receives a generated order number.

The application derives the number from the current timestamp and uses the resulting value as a short order reference.

This gives the store a simple identifier for communicating about orders.

---

# 👨‍💼 Admin Architecture

The admin interface is integrated into the same application.

Conceptually:

```text
Admin
 │
 ▼
Admin Login
 │
 ▼
Session Validation
 │
 ▼
Admin Dashboard
 │
 ▼
Firestore Orders
 │
 ▼
Display Orders
```

The admin dashboard reads order data from Firestore and displays customer/order information.

---

# 🌐 Deployment

The application can be deployed as a static website because the frontend is client-side and Firebase provides the backend services.

Recommended deployment:

**GitHub Pages**

Repository:

```text
grocery-project
```

GitHub username:

```text
gudipatimanikesh
```

Expected website:

```text
https://gudipatimanikesh.github.io/grocery-project/
```

---

# 🚀 GitHub Pages Deployment

## 1. Create Repository

Create:

```text
grocery-project
```

under:

```text
gudipatimanikesh
```

---

## 2. Add Application

The main application file should be:

```text
index.html
```

Repository structure:

```text
grocery-project/
│
├── index.html
└── README.md
```

---

## 3. Enable GitHub Pages

Go to:

```text
Repository
   ↓
Settings
   ↓
Pages
```

Configure:

```text
Source: Deploy from a branch
Branch: main
Folder: / (root)
```

After deployment:

```text
https://gudipatimanikesh.github.io/grocery-project/
```

---

# 🔥 Firebase + GitHub Pages

When deployed on GitHub Pages, the application runs under HTTPS.

This is important for browser functionality such as:

* Google Authentication
* Browser Geolocation

The GitHub Pages domain should also be configured in Firebase Authentication's authorized domains.

Recommended domain:

```text
gudipatimanikesh.github.io
```

---

# 🔒 Security Considerations

The application is intended as a lightweight client-side grocery ordering system.

Important production considerations include:

### Firebase Security Rules

Firestore security rules should be configured to ensure that customers cannot arbitrarily modify or delete other customers' data.

Recommended principle:

```text
Customer
   │
   ├── Read permitted product data
   │
   └── Create/read permitted own order data
```

Administrative access should be restricted separately.

---

### Admin Authentication

The current application contains client-side admin authentication logic.

For a production-scale application, administrative authorization should preferably be enforced using backend/Firebase security rules rather than relying only on client-side checks.

---

### Firebase Configuration

Firebase web configuration values are intended to be used by client applications.

However, sensitive server-side credentials such as:

```text
Firebase Admin SDK private keys
Service account credentials
Private secrets
```

should never be placed inside `index.html` or committed to GitHub.

---

# 📱 Responsive Design

The application is intended to support:

```text
📱 Mobile
💻 Laptop
🖥️ Desktop
📲 Tablet
```

This is particularly important because grocery ordering is expected to be primarily used from mobile devices.

---

# 🎯 Project Goals

The project aims to provide a simple digital ordering system for a local grocery store without requiring a traditional backend server.

The architecture reduces infrastructure requirements by using:

```text
Static Frontend
      +
Firebase Backend
      +
WhatsApp Communication
```

This allows the store to maintain an online product catalogue and receive customer orders without requiring a complex e-commerce platform.

---

# 🏛️ Architecture Summary

| Layer               | Technology              | Responsibility             |
| ------------------- | ----------------------- | -------------------------- |
| Presentation        | HTML5                   | Page structure             |
| Styling             | CSS3                    | Responsive UI              |
| Application Logic   | JavaScript              | Client-side functionality  |
| Authentication      | Firebase Authentication | Google Sign-In             |
| Database            | Cloud Firestore         | Products, users and orders |
| Location            | Browser Geolocation API | Customer coordinates       |
| Maps                | Google Maps URL         | Location navigation        |
| Communication       | WhatsApp `wa.me`        | Order communication        |
| Hosting             | GitHub Pages            | Static website hosting     |
| Browser Persistence | LocalStorage            | Cart/profile persistence   |
| Session State       | SessionStorage          | Admin session              |

---

# 🗂️ Project Structure

```text
grocery-project/
│
├── index.html
│   ├── HTML
│   ├── CSS
│   └── JavaScript
│
└── README.md
```

The current implementation intentionally keeps the project lightweight by using a single-page architecture.

---

# 🛠️ Local Development

Because Firebase Authentication and browser geolocation work best in a secure context, development is preferably performed through a local development server rather than opening the HTML directly using:

```text
file:///
```

For example, the project can be served through a local HTTP server during development.

For production:

```text
GitHub Pages
        ↓
HTTPS
        ↓
Firebase + Browser APIs
```

---

# 🔮 Future Improvements

Potential future enhancements include:

* 🛍️ Product inventory management
* 📦 Order status updates
* 🔔 Customer order notifications
* 💳 Online payment integration
* 🧾 Digital invoices
* 📊 Sales dashboard
* 📈 Revenue analytics
* 👥 Customer management
* 🏷️ Discount/coupon system
* 📦 Stock availability
* 🖼️ Product image management
* 🔎 Advanced product search
* 📱 Progressive Web App (PWA)
* 🔔 Push notifications
* 🧑‍💼 Role-based admin access
* 🔐 Firebase Security Rules hardening
* 📊 Admin analytics
* 🧾 Order history for customers

---

# 📌 Current Architecture at a Glance

```text
                         SHARMA KIRANA STORE
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │     GitHub Pages    │
                       │       HTTPS         │
                       └──────────┬──────────┘
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │     index.html      │
                       │                     │
                       │ HTML + CSS + JS     │
                       └──────────┬──────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
       ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
       │  Firebase   │     │  Browser    │     │  WhatsApp   │
       │             │     │  APIs       │     │             │
       │ Auth        │     │             │     │ Order       │
       │ Firestore   │     │ Geolocation │     │ Messaging   │
       └──────┬──────┘     └─────────────┘     └─────────────┘
              │
              ▼
       ┌─────────────────┐
       │    Firestore    │
       │                 │
       │  Products       │
       │  Users          │
       │  Orders         │
       └────────┬────────┘
                │
                ▼
       ┌─────────────────┐
       │  Admin Panel    │
       │                 │
       │  Orders         │
       │  Customers      │
       │  Status         │
       └─────────────────┘
```

---

# 👨‍💻 Developer

**Gudipati Manikesh**

GitHub:

`gudipatimanikesh`

Portfolio:

`https://gudipatimanikesh.github.io`

Project:

`https://gudipatimanikesh.github.io/grocery-project/`

---

# 📄 License

This project is currently maintained as a custom project for **Sharma Kirana Store**.

If you intend to make the project open-source, add an appropriate license such as MIT License and update this section accordingly.

---

## ⭐ Project Highlights

* Serverless architecture
* Firebase-powered authentication
* Cloud Firestore database
* Google Sign-In
* Dynamic product catalogue
* Shopping cart
* Checkout workflow
* Browser geolocation
* WhatsApp integration
* Admin order dashboard
* Responsive UI
* GitHub Pages deployment
* HTTPS-ready architecture

---

**Built with HTML, CSS, JavaScript, Firebase and GitHub Pages.**
