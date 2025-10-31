SlotSwapper - Peer-to-Peer Time-Slot Scheduling
SlotSwapper is a full-stack web application that enables users to swap time slots with each other. Users can mark their busy calendar slots as "swappable" and request to exchange them with other users' available slots.

🎯 Features
User Authentication: Secure JWT-based authentication system with signup and login
Calendar Management: Create, view, update, and delete personal calendar events
Swap Status Management: Mark events as BUSY, SWAPPABLE, or SWAP_PENDING
Marketplace: Browse and discover swappable slots from other users
Swap Requests: Send and receive slot swap requests with Accept/Reject functionality
Real-time Updates: Dynamic state management ensures UI reflects changes immediately
Protected Routes: Authentication-based route protection
Responsive Design: Mobile-friendly interface
🛠️ Technology Stack
Frontend
React.js 18
React Router v6 for routing
Axios for API calls
Context API for state management
CSS3 for styling
Backend
Node.js with Express.js
MySQL database
JWT for authentication
bcrypt.js for password hashing
RESTful API architecture
DevOps
Docker & Docker Compose for containerization
MySQL 8.0 container
📁 Project Structure
slotswapper/
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── database.js          # Database connection & initialization
│   │   ├── middleware/
│   │   │   └── auth.js              # JWT authentication middleware
│   │   ├── controllers/
│   │   │   ├── authController.js    # Authentication logic
│   │   │   ├── eventController.js   # Event CRUD operations
│   │   │   └── swapController.js    # Swap request logic
│   │   ├── routes/
│   │   │   ├── auth.js              # Auth routes
│   │   │   ├── events.js            # Event routes
│   │   │   └── swaps.js             # Swap routes
│   │   └── server.js                # Entry point
│   ├── Dockerfile
│   ├── package.json
│   └── .env.example
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Auth/                # Login & SignUp components
│   │   │   ├── Calendar/            # Dashboard & EventForm
│   │   │   ├── Marketplace/         # Marketplace & SwapModal
│   │   │   ├── Requests/            # Request management
│   │   │   └── Layout/              # Navbar & ProtectedRoute
│   │   ├── context/
│   │   │   └── AuthContext.jsx      # Authentication context
│   │   ├── services/
│   │   │   └── api.js               # API service layer
│   │   ├── App.jsx
│   │   └── index.js
│   ├── Dockerfile
│   └── package.json
│
├── docker-compose.yml
├── .gitignore
└── README.md
🚀 Getting Started
Prerequisites
Node.js (v16 or higher)
MySQL (v8.0 or higher)
Docker & Docker Compose (optional, for containerized setup)
Option 1: Docker Setup (Recommended)
Clone the repository
bash
git clone <repository-url>
cd slotswapper
Start all services with Docker Compose
bash
docker-compose up --build
This will start:

MySQL database on port 3306
Backend API on port 5000
Frontend application on port 3000
Access the application
Frontend: http://localhost:3000
Backend API: http://localhost:5000
Option 2: Manual Setup
Backend Setup
Navigate to backend directory
bash
cd backend
Install dependencies
bash
npm install
Configure environment variables
bash
cp .env.example .env
Edit .env with your database credentials:

PORT=5000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=slotswapper
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
JWT_EXPIRE=7d
NODE_ENV=development
Create MySQL database
sql
CREATE DATABASE slotswapper;
Start the backend server
bash
npm run dev
The backend will automatically create the required tables on first run.

Frontend Setup
Navigate to frontend directory
bash
cd frontend
Install dependencies
bash
npm install
Create environment file (optional)
bash
# Create .env file
REACT_APP_API_URL=http://localhost:5000/api
Start the development server
bash
npm start
The application will open at http://localhost:3000

📡 API Endpoints
Authentication Endpoints
Method	Endpoint	Description	Auth Required
POST	/api/auth/signup	Register new user	No
POST	/api/auth/login	Login user	No
GET	/api/auth/profile	Get user profile	Yes
Signup Request Body:

json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
Login Request Body:

json
{
  "email": "john@example.com",
  "password": "password123"
}
Event Endpoints
Method	Endpoint	Description	Auth Required
GET	/api/events	Get user's events	Yes
POST	/api/events	Create new event	Yes
PUT	/api/events/:id	Update event	Yes
DELETE	/api/events/:id	Delete event	Yes
Create Event Request Body:

json
{
  "title": "Team Meeting",
  "startTime": "2024-01-15T10:00:00",
  "endTime": "2024-01-15T11:00:00",
  "status": "BUSY"
}
Swap Endpoints
Method	Endpoint	Description	Auth Required
GET	/api/swappable-slots	Get all swappable slots	Yes
POST	/api/swap-request	Create swap request	Yes
POST	/api/swap-response/:requestId	Accept/Reject swap	Yes
GET	/api/my-requests	Get user's swap requests	Yes
Create Swap Request Body:

json
{
  "mySlotId": 1,
  "theirSlotId": 5
}
Swap Response Body:

json
{
  "accept": true
}
Authentication
All protected endpoints require a JWT token in the Authorization header:

Authorization: Bearer <your_jwt_token>
🎨 Design Choices
Backend Architecture
Database Schema:
users: Stores user credentials and profile information
events: Stores calendar events with status (BUSY, SWAPPABLE, SWAP_PENDING)
swap_requests: Tracks swap requests between users with status (PENDING, ACCEPTED, REJECTED)
Transaction Safety: The swap logic uses MySQL transactions to ensure data consistency when:
Creating swap requests (updating both slots to SWAP_PENDING)
Accepting swaps (exchanging ownership of slots)
Rejecting swaps (reverting slots back to SWAPPABLE)
Security:
Passwords hashed using bcrypt with salt rounds
JWT tokens for stateless authentication
Middleware protection for all sensitive endpoints
Input validation on all endpoints
Frontend Architecture
State Management: Context API for global authentication state, local state for component-specific data
Routing: React Router with protected routes that redirect unauthenticated users
API Layer: Centralized API service with Axios interceptors for:
Automatic token attachment to requests
Global error handling
Automatic redirect on authentication failure
User Experience:
Loading states for async operations
Error messages for failed operations
Success confirmations for critical actions
Responsive design for mobile devices
🔑 Key Implementation Details
The Swap Logic
The core swap functionality follows this flow:

Creating a Swap Request:
Validates both slots exist and are SWAPPABLE
Creates a PENDING swap request
Updates both slots to SWAP_PENDING (prevents double-booking)
Accepting a Swap:
Exchanges the user_id of both events (ownership transfer)
Sets both slots back to BUSY status
Marks request as ACCEPTED
Rejecting a Swap:
Reverts both slots to SWAPPABLE status
Marks request as REJECTED
🧪 Testing
Running Tests
Backend tests (when implemented):

bash
cd backend
npm test
Frontend tests (when implemented):

bash
cd frontend
npm test
🚢 Deployment
Backend Deployment (Render/Heroku)
Set environment variables in your hosting platform
Connect your GitHub repository
Deploy from main branch
Frontend Deployment (Vercel/Netlify)
Build the project:
bash
cd frontend
npm run build
Deploy the build folder to your hosting platform
Set REACT_APP_API_URL environment variable to your backend URL
🐛 Known Issues & Limitations
No real-time notifications (WebSocket integration could be added)
Limited test coverage (unit and integration tests to be added)
No email notifications for swap requests
Time zone handling uses browser local time
No calendar view (currently list-based)
🔮 Future Enhancements
 Real-time notifications using WebSockets
 Email notifications for swap requests
 Calendar grid view
 Recurring events support
 Swap history and analytics
 User profiles with avatars
 Search and filter functionality
 Mobile app (React Native)
 Time zone support
 Event categories/tags
🤝 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

📄 License
This project is licensed under the MIT License.

👨‍💻 Author
Your Name - [Your Email]

🙏 Acknowledgments
Built as part of a technical assessment
Inspired by the need for flexible scheduling solutions
Note: This is a demonstration project. For production use, additional security measures, comprehensive testing, and performance optimizations should be implemented.

