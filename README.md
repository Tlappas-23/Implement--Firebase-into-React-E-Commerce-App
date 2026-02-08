# Firebase E-Commerce App (React + Vite)

React e‑commerce app with Firebase Authentication and Firestore for product and order management.

## Features
- Email/password registration, login, logout
- User profile CRUD (name, address)
- Product CRUD (create, read, update, delete)
- Cart with checkout
- Order history + order detail views

## Tech Stack
- React 19 + Vite
- Firebase Auth + Firestore
- React Bootstrap

## Setup
1. Install dependencies
   ```bash
   npm install
   ```

2. Create a Firebase project
   - Enable **Authentication** (Email/Password)
   - Enable **Firestore Database**
   - Add a **Web App** and copy the config values

3. Create `.env`
   - Copy `.env.example` to `.env`
   - Paste your Firebase config values

4. (Optional) Firestore rules
   ```rules
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /products/{productId} {
         allow read: if true;
         allow write: if request.auth != null;
       }
       match /users/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
       match /orders/{orderId} {
         allow create: if request.auth != null && request.auth.uid == request.resource.data.userId;
         allow read, update, delete: if request.auth != null && request.auth.uid == resource.data.userId;
       }
     }
   }
   ```

5. Seed products
   - Use **Add Product** in the app, or
   - Create `products` docs in Firestore manually:
     - `title` (string)
     - `price` (number)
     - `description` (string)
     - `category` (string)
     - `image` (string URL)

6. Run the app
   ```bash
   npm run dev
   ```

## Notes
- Deleting an account may require a recent login (Firebase security rule).
- Products are read from Firestore (FakeStore API is no longer used).
