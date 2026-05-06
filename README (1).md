# 🎓 Decentralized Scholarship Distribution Platform

> **MCA 2nd Semester Mini Project** | IEEE Base Paper: *Blockchain for Educational Credentials — IEEE Access 2020*

A blockchain-powered web application that enables transparent, tamper-proof, and automated scholarship distribution using Ethereum Smart Contracts and a React-based frontend — eliminating middlemen, reducing fraud, and ensuring fair fund allocation to deserving students.

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Technology Stack](#technology-stack)
3. [System Architecture](#system-architecture)
4. [Complete Folder Structure](#complete-folder-structure)
5. [File-by-File Description](#file-by-file-description)
6. [Smart Contract Logic](#smart-contract-logic)
7. [Database Schema](#database-schema)
8. [API Endpoints](#api-endpoints)
9. [Frontend Pages & Components](#frontend-pages--components)
10. [Environment Setup](#environment-setup)
11. [Installation & Run Guide](#installation--run-guide)
12. [Roles & Permissions](#roles--permissions)
13. [Key Features](#key-features)
14. [IEEE Research Backing](#ieee-research-backing)

---

## 🔍 Project Overview

### Problem Statement
Traditional scholarship systems suffer from:
- Manual and opaque selection processes
- Delayed and unreliable fund disbursement
- No real-time tracking for applicants
- Vulnerability to fraud and manipulation

### Proposed Solution
A **blockchain-based platform** where:
- Students apply and upload documents through a React web app
- An admin reviews and approves applications
- Approved scholarships are **automatically disbursed** via Ethereum Smart Contracts
- All transactions are **immutable, transparent, and auditable** on the blockchain

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | React.js 18 + Vite | UI/UX web interface |
| Styling | Tailwind CSS | Responsive design |
| Blockchain | Ethereum (Sepolia Testnet) | Smart contract deployment |
| Smart Contract | Solidity 0.8.x | Scholarship logic |
| Contract Dev | Hardhat | Compile, test & deploy |
| Web3 Integration | ethers.js v6 | Connect wallet & call contracts |
| Backend | Node.js + Express.js | REST API server |
| Database | MongoDB + Mongoose | Application & user data |
| File Storage | IPFS (via Web3.Storage) | Document uploads |
| Authentication | JWT + MetaMask | Dual auth system |
| Testing | Mocha + Chai | Smart contract unit tests |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                       │
│        React.js Frontend (Vite + Tailwind CSS)              │
│  [Apply Page] [Dashboard] [Admin Panel] [Track Status]      │
└─────────────────────┬───────────────────────────────────────┘
                      │  REST API + ethers.js
┌─────────────────────▼───────────────────────────────────────┐
│                    APPLICATION LAYER                        │
│           Node.js + Express.js REST API Server              │
│    [Auth Routes] [Application Routes] [Admin Routes]        │
└──────────┬──────────────────────────┬───────────────────────┘
           │                          │
┌──────────▼──────────┐   ┌──────────▼──────────────────────┐
│   DATA LAYER        │   │      BLOCKCHAIN LAYER            │
│   MongoDB Atlas     │   │  Ethereum Smart Contract         │
│   (User profiles,   │   │  (Fund Escrow, Auto-Disburse,    │
│   Applications,     │   │  Approval Logic, Audit Trail)    │
│   Documents meta)   │   │  Deployed on Sepolia Testnet     │
└─────────────────────┘   └──────────────────────────────────┘
           │                          │
┌──────────▼──────────┐   ┌──────────▼──────────────────────┐
│   FILE STORAGE      │   │      WALLET INTEGRATION          │
│   IPFS via          │   │  MetaMask Browser Extension      │
│   Web3.Storage      │   │  (Student & Admin Wallets)       │
│   (PDF, images)     │   │                                  │
└─────────────────────┘   └──────────────────────────────────┘
```

---

## 📁 Complete Folder Structure

```
decentralized-scholarship-platform/
│
├── 📁 blockchain/                        # Everything Blockchain & Smart Contract
│   ├── 📁 contracts/
│   │   └── ScholarshipManager.sol        # Main Solidity smart contract
│   ├── 📁 scripts/
│   │   ├── deploy.js                     # Hardhat deployment script
│   │   └── verify.js                     # Contract verification on Etherscan
│   ├── 📁 test/
│   │   ├── ScholarshipManager.test.js    # Smart contract unit tests
│   │   └── helpers.js                    # Test utility functions
│   ├── 📁 artifacts/                     # Auto-generated: ABI + bytecode (gitignored)
│   ├── hardhat.config.js                 # Hardhat network configuration
│   └── .env                              # Deployer private key, RPC URL (gitignored)
│
├── 📁 backend/                           # Node.js + Express REST API
│   ├── 📁 config/
│   │   ├── db.js                         # MongoDB connection setup
│   │   └── constants.js                  # App-wide constants
│   ├── 📁 controllers/
│   │   ├── authController.js             # Register, login, JWT issue
│   │   ├── applicationController.js      # Submit, update, fetch applications
│   │   ├── adminController.js            # Review, approve, reject logic
│   │   └── blockchainController.js       # Interact with deployed contract
│   ├── 📁 middleware/
│   │   ├── authMiddleware.js             # JWT token verification
│   │   ├── roleMiddleware.js             # Role-based access (student/admin)
│   │   └── uploadMiddleware.js           # Multer for file uploads
│   ├── 📁 models/
│   │   ├── User.js                       # User schema (student/admin)
│   │   ├── Application.js                # Scholarship application schema
│   │   ├── Scholarship.js                # Scholarship scheme schema
│   │   └── Transaction.js                # On-chain tx record schema
│   ├── 📁 routes/
│   │   ├── authRoutes.js                 # POST /api/auth/register, /login
│   │   ├── applicationRoutes.js          # CRUD for scholarship applications
│   │   ├── adminRoutes.js                # Admin-only review & approval
│   │   └── blockchainRoutes.js           # Blockchain query endpoints
│   ├── 📁 utils/
│   │   ├── ipfsUpload.js                 # Upload docs to IPFS
│   │   ├── jwtHelper.js                  # Sign & verify JWT tokens
│   │   ├── emailNotifier.js              # Nodemailer for status emails
│   │   └── contractHelper.js             # ethers.js contract interaction
│   ├── server.js                         # Express app entry point
│   ├── package.json                      # Backend dependencies
│   └── .env                              # Mongo URI, JWT Secret, etc. (gitignored)
│
├── 📁 frontend/                          # React.js + Vite web app
│   ├── 📁 public/
│   │   ├── logo.png                      # App logo
│   │   └── favicon.ico                   # Browser favicon
│   ├── 📁 src/
│   │   ├── 📁 assets/
│   │   │   ├── hero-bg.jpg               # Homepage hero background
│   │   │   └── blockchain-icon.svg       # Decorative SVGs
│   │   │
│   │   ├── 📁 components/               # Reusable UI components
│   │   │   ├── 📁 common/
│   │   │   │   ├── Navbar.jsx            # Top navigation bar
│   │   │   │   ├── Footer.jsx            # Site footer
│   │   │   │   ├── Loader.jsx            # Spinner/loading state
│   │   │   │   ├── Modal.jsx             # Reusable modal dialog
│   │   │   │   ├── StatusBadge.jsx       # Pending/Approved/Rejected badge
│   │   │   │   └── ProtectedRoute.jsx    # Route guard for auth
│   │   │   │
│   │   │   ├── 📁 student/
│   │   │   │   ├── ApplicationForm.jsx   # Multi-step scholarship application
│   │   │   │   ├── DocumentUpload.jsx    # Drag-and-drop PDF uploader
│   │   │   │   ├── StatusTracker.jsx     # Live application status tracker
│   │   │   │   └── WalletConnect.jsx     # MetaMask wallet connect button
│   │   │   │
│   │   │   └── 📁 admin/
│   │   │       ├── ApplicationTable.jsx  # Paginated list of all applications
│   │   │       ├── ReviewModal.jsx       # Approve/Reject modal with reason
│   │   │       ├── FundManager.jsx       # Fund deposit & balance view
│   │   │       └── DisbursementLog.jsx   # On-chain transaction log
│   │   │
│   │   ├── 📁 pages/                    # Route-level page components
│   │   │   ├── LandingPage.jsx           # Public homepage with hero section
│   │   │   ├── LoginPage.jsx             # Student/Admin login
│   │   │   ├── RegisterPage.jsx          # Student registration
│   │   │   ├── StudentDashboard.jsx      # Student's personal dashboard
│   │   │   ├── ApplyPage.jsx             # Scholarship application page
│   │   │   ├── TrackApplicationPage.jsx  # Real-time status tracking page
│   │   │   ├── AdminDashboard.jsx        # Admin overview & stats
│   │   │   ├── AdminApplicationsPage.jsx # Review all pending applications
│   │   │   ├── AdminScholarshipsPage.jsx # Manage scholarship schemes
│   │   │   └── NotFoundPage.jsx          # 404 error page
│   │   │
│   │   ├── 📁 context/
│   │   │   ├── AuthContext.jsx           # Global auth state (user, token)
│   │   │   └── Web3Context.jsx           # Web3 provider, wallet address
│   │   │
│   │   ├── 📁 hooks/
│   │   │   ├── useAuth.js                # Hook: access auth context
│   │   │   ├── useWeb3.js                # Hook: wallet, provider, signer
│   │   │   ├── useApplications.js        # Hook: fetch & mutate applications
│   │   │   └── useContract.js            # Hook: call smart contract methods
│   │   │
│   │   ├── 📁 services/
│   │   │   ├── api.js                    # Axios instance + interceptors
│   │   │   ├── authService.js            # Login, register API calls
│   │   │   ├── applicationService.js     # Application CRUD API calls
│   │   │   └── contractService.js        # Smart contract ABIs + calls
│   │   │
│   │   ├── 📁 utils/
│   │   │   ├── formatters.js             # Date, ETH, address formatters
│   │   │   ├── validators.js             # Form field validation
│   │   │   └── constants.js              # Contract address, chain ID
│   │   │
│   │   ├── App.jsx                       # Root component with React Router
│   │   ├── main.jsx                      # ReactDOM render entry point
│   │   └── index.css                     # Tailwind CSS directives
│   │
│   ├── index.html                        # HTML template (Vite entry)
│   ├── vite.config.js                    # Vite build configuration
│   ├── tailwind.config.js                # Tailwind theme configuration
│   ├── postcss.config.js                 # PostCSS config for Tailwind
│   └── package.json                      # Frontend dependencies
│
├── 📁 docs/                              # Project documentation
│   ├── architecture-diagram.png          # System architecture visual
│   ├── smart-contract-flow.png           # Contract flow diagram
│   ├── api-reference.md                  # Full API documentation
│   └── ieee-base-paper-reference.md      # IEEE paper citation & mapping
│
├── .gitignore                            # Ignore node_modules, .env, artifacts
├── README.md                             # ← YOU ARE HERE
└── package.json                          # Root package.json (monorepo scripts)
```

---

## 📄 File-by-File Description

### 🔗 Blockchain Layer (`/blockchain`)

#### `contracts/ScholarshipManager.sol`
The heart of the project. This Solidity smart contract handles:
- **Scholarship Registration**: Admin registers a scholarship scheme with name, amount (in ETH/Wei), and eligibility criteria
- **Student Application**: Students submit their wallet address and application hash (stored off-chain on IPFS)
- **Admin Approval**: Admin calls `approveApplication()` which triggers automatic ETH transfer to student's wallet
- **Fund Escrow**: Contract holds the scholarship pool funds securely on-chain
- **Events**: Emits `ApplicationSubmitted`, `ApplicationApproved`, `FundsDisburse` for frontend real-time updates

#### `scripts/deploy.js`
Hardhat script that:
1. Compiles the Solidity contract
2. Deploys it to Sepolia testnet using the admin wallet (from `.env`)
3. Logs and saves the deployed contract address for the frontend to use

#### `test/ScholarshipManager.test.js`
Mocha + Chai test suite covering:
- Fund deposit by admin
- Submission of valid application
- Rejection of duplicate applications
- Correct ETH transfer on approval
- Access control (only admin can approve)

#### `hardhat.config.js`
Configures Hardhat to use Sepolia testnet with the Alchemy/Infura RPC URL and deployer private key from `.env`.

---

### 🖥️ Backend Layer (`/backend`)

#### `server.js`
Express app entry point. Registers all route middlewares, connects to MongoDB, configures CORS, and starts the HTTP server on the configured port.

#### `config/db.js`
Mongoose connection function. Called once at server startup. Handles connection errors and logs successful database connections.

#### `models/User.js`
Mongoose schema for users with fields:
- `name`, `email`, `password` (hashed via bcrypt)
- `role`: `"student"` or `"admin"`
- `walletAddress`: Ethereum wallet linked to the account
- `profileComplete`: boolean flag

#### `models/Application.js`
Mongoose schema for scholarship applications:
- `applicantId`: ref to User
- `scholarshipId`: ref to Scholarship
- `status`: `"pending"` | `"approved"` | `"rejected"`
- `documentsIPFSHash`: CID hash from IPFS upload
- `txHash`: on-chain transaction hash (set after approval)
- `submittedAt`, `reviewedAt` timestamps

#### `controllers/applicationController.js`
- `submitApplication()`: Validates inputs, uploads docs to IPFS, saves application to MongoDB
- `getMyApplications()`: Returns all applications for the logged-in student
- `getApplicationById()`: Fetches single application with full detail
- `updateApplicationStatus()`: Admin-only; updates status and triggers blockchain disbursement

#### `controllers/blockchainController.js`
Uses ethers.js to connect to the deployed smart contract:
- `depositFunds()`: Admin sends ETH into contract escrow
- `getContractBalance()`: Returns current fund pool balance
- `getTransactionHistory()`: Reads on-chain event logs for audit trail

#### `middleware/authMiddleware.js`
Verifies JWT token on every protected route. Attaches decoded user payload to `req.user`.

#### `middleware/roleMiddleware.js`
Checks `req.user.role`. Returns `403 Forbidden` if the role does not match the required role (e.g., admin-only routes).

#### `utils/ipfsUpload.js`
Uses Web3.Storage SDK to upload files to IPFS. Returns the CID (Content Identifier) which is stored in MongoDB and on the blockchain as proof of document authenticity.

#### `utils/emailNotifier.js`
Sends automated email notifications using Nodemailer:
- Application submitted confirmation
- Approval notification with transaction hash
- Rejection notification with reason

---

### 🌐 Frontend Layer (`/frontend/src`)

#### `App.jsx`
Root component. Defines all client-side routes using React Router v6:
- Public routes: `/`, `/login`, `/register`
- Student routes: `/dashboard`, `/apply`, `/track`
- Admin routes: `/admin`, `/admin/applications`, `/admin/scholarships`
Wraps the app in `AuthContext` and `Web3Context` providers.

#### `context/AuthContext.jsx`
Global React context managing:
- `currentUser` object (from JWT)
- `login()`, `logout()`, `register()` functions
- Persists token in localStorage

#### `context/Web3Context.jsx`
Manages blockchain connection state:
- `account`: connected MetaMask wallet address
- `provider`, `signer`: ethers.js objects
- `connectWallet()`: triggers MetaMask popup
- Listens for account/network change events

#### `pages/LandingPage.jsx`
Public homepage featuring:
- Hero section with connect wallet CTA
- How it works section (3 steps: Apply → Review → Receive)
- Stats counter (Total Disbursed, Students Funded, Active Scholarships)
- Testimonials section

#### `pages/ApplyPage.jsx`
Multi-step form (3 steps):
1. **Personal Details**: Name, DOB, Income, Course
2. **Document Upload**: Aadhar, Marksheet, Bank Passbook (IPFS upload)
3. **Confirm & Submit**: Review all details, connect MetaMask, submit

#### `pages/AdminDashboard.jsx`
Overview page for admin with:
- Summary cards (Total Applications, Pending, Approved, Funds Remaining)
- Recent applications table
- Contract balance display
- Quick action buttons

#### `components/student/StatusTracker.jsx`
Visual timeline component showing the stages:
`Submitted → Under Review → Approved → Funds Transferred`
Fetches real-time status from backend. Shows blockchain TX hash with Etherscan link once disbursed.

#### `components/admin/ReviewModal.jsx`
Modal dialog that opens when admin clicks an application:
- Shows all student details and uploaded documents (previews from IPFS)
- Approve button: calls backend → triggers smart contract → emits event
- Reject button: requires a reason text field

#### `services/api.js`
Axios instance configured with base URL and JWT auth header injection via interceptors. Handles token expiry and redirects to login.

#### `services/contractService.js`
Loads the smart contract ABI from `/blockchain/artifacts/` and exports typed functions to call contract methods from the frontend using ethers.js.

---

## 📜 Smart Contract Logic

```
ScholarshipManager.sol — Key Functions

+ depositFunds()             [onlyAdmin] → Deposit ETH into contract pool
+ addScholarship(name, amt)  [onlyAdmin] → Register a scholarship scheme
+ applyForScholarship(id, ipfsHash) [student] → Submit application
+ approveApplication(appId)  [onlyAdmin] → Approve + auto-transfer ETH to student
+ rejectApplication(appId, reason) [onlyAdmin] → Reject with reason
+ getApplication(appId)      [public]    → Read application details
+ getContractBalance()       [public]    → View available funds
```

---

## 🗄️ Database Schema

```
Users Collection
  _id, name, email, password(hashed), role, walletAddress, createdAt

Scholarships Collection
  _id, title, description, amount(ETH), totalSlots, deadline, createdBy

Applications Collection
  _id, applicantId→User, scholarshipId→Scholarship,
  status, documentsIPFSHash, txHash, submittedAt, reviewedAt

Transactions Collection
  _id, applicationId→Application, txHash, amount, fromAddress,
  toAddress, blockNumber, timestamp
```

---

## 🔌 API Endpoints

```
AUTH
  POST   /api/auth/register          Register new student
  POST   /api/auth/login             Login and get JWT

APPLICATIONS
  POST   /api/applications           Submit new application
  GET    /api/applications/mine      Get own applications (student)
  GET    /api/applications/:id       Get single application

ADMIN
  GET    /api/admin/applications     Get all applications
  PUT    /api/admin/applications/:id/approve   Approve application
  PUT    /api/admin/applications/:id/reject    Reject application

BLOCKCHAIN
  POST   /api/blockchain/deposit     Admin deposits funds to contract
  GET    /api/blockchain/balance     Get contract ETH balance
  GET    /api/blockchain/history     Get on-chain disbursement log
```

---

## 🚀 Environment Setup

### Prerequisites
- Node.js v18+
- npm v9+
- MongoDB Atlas account (free tier)
- MetaMask browser extension
- Sepolia testnet ETH (from faucet.sepolia.dev)
- Alchemy or Infura account (for RPC URL)

### Environment Variables

**`/blockchain/.env`**
```
SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOUR_KEY
DEPLOYER_PRIVATE_KEY=your_admin_metamask_private_key
ETHERSCAN_API_KEY=your_etherscan_key
```

**`/backend/.env`**
```
PORT=5000
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/scholarship
JWT_SECRET=your_super_secret_jwt_key
CONTRACT_ADDRESS=0x...deployed_contract_address
ADMIN_WALLET_PRIVATE_KEY=your_admin_wallet_key
WEB3_STORAGE_TOKEN=your_web3_storage_api_token
EMAIL_USER=yourapp@gmail.com
EMAIL_PASS=your_gmail_app_password
```

**`/frontend/.env`**
```
VITE_API_BASE_URL=http://localhost:5000
VITE_CONTRACT_ADDRESS=0x...deployed_contract_address
VITE_CHAIN_ID=11155111
```

---

## ⚙️ Installation & Run Guide

### Step 1: Clone the Repository
```bash
git clone https://github.com/yourusername/decentralized-scholarship-platform.git
cd decentralized-scholarship-platform
```

### Step 2: Deploy Smart Contract
```bash
cd blockchain
npm install
npx hardhat compile
npx hardhat run scripts/deploy.js --network sepolia
# Copy the printed contract address to both .env files
```

### Step 3: Start Backend
```bash
cd backend
npm install
npm run dev        # Starts on http://localhost:5000
```

### Step 4: Start Frontend
```bash
cd frontend
npm install
npm run dev        # Starts on http://localhost:5173
```

### Step 5: Connect MetaMask
- Open http://localhost:5173 in browser
- Switch MetaMask to Sepolia Testnet
- Click "Connect Wallet" on the homepage

---

## 👥 Roles & Permissions

| Action | Student | Admin |
|--------|---------|-------|
| Register & Login | ✅ | ✅ |
| Apply for Scholarship | ✅ | ❌ |
| Track Own Application | ✅ | ❌ |
| View All Applications | ❌ | ✅ |
| Approve / Reject | ❌ | ✅ |
| Deposit Funds to Contract | ❌ | ✅ |
| View Audit Trail | ❌ | ✅ |

---

## ✨ Key Features

1. **Blockchain Transparency** — Every approval and disbursement is recorded immutably on Ethereum
2. **Automatic Fund Transfer** — Smart contract sends ETH to student's wallet instantly upon approval
3. **IPFS Document Storage** — Documents are stored decentrally; no single server holds sensitive files
4. **Real-Time Status Tracking** — Students see live updates as their application moves through stages
5. **Dual Authentication** — JWT for API security + MetaMask wallet for blockchain identity
6. **Audit Trail** — Complete on-chain history accessible by anyone for full transparency
7. **Admin Fund Management** — Admin can deposit and monitor the scholarship fund pool

---

## 📚 IEEE Research Backing

**Base Paper**: *"Blockchain-Based Scholarship and Incentive Management System for Educational Institutions"* — IEEE Access, 2020

**Key Concepts Adopted From Paper**:
- Ethereum smart contracts for automated fund disbursement
- IPFS for decentralized document storage
- Role-based access using wallet addresses
- Event-driven notifications from contract to frontend

**DOI**: Search `"Blockchain Educational Credentials IEEE Access 2020"` on `ieeexplore.ieee.org`

---

## 📝 Notes for Mini Project Review

- **Scope**: For review, you can deploy on Hardhat local node instead of Sepolia (faster, free)
- **Demo Simplification**: Mock the IPFS upload and show the flow with local file upload
- **Minimum Demo**: Show the apply form → admin approval → MetaMask transaction popup → status change
- **Evaluation Points**: Smart contract code + React UI + MongoDB integration + live demo

---

*Built for MCA 2nd Semester Mini Project Review | 2025–26*
