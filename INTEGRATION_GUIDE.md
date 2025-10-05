# SmartBite Frontend + ServeSoft Backend Integration

This project integrates the SmartBite React frontend with the ServeSoft PHP backend.

## Architecture Overview

- **Frontend**: SmartBite React application (Vite + React + TypeScript)
- **Backend**: ServeSoft PHP REST API with MySQL database
- **Database**: MySQL database named `SERVESOFT`

## Setup Instructions

### 1. Database Setup

Make sure your MySQL database is running and the `SERVESOFT` database exists:

```bash
# Access MySQL
mysql -u root -p

# The database should already exist from ServeSoft setup
USE SERVESOFT;
```

### 2. Backend Setup (ServeSoft)

The ServeSoft PHP backend should be accessible at:
```
http://localhost/github%20REpo/servesoft
```

Make sure your Apache/XAMPP server is running and the files are in the correct location.

### 3. Frontend Setup (SmartBite)

Navigate to the SmartBite directory and install dependencies:

```bash
cd "github REpo/smartbite"
npm install
```

Start the development server:

```bash
npm run dev
```

The application will be available at `http://localhost:5173`

## How It Works

### Authentication Flow

1. User logs in through SmartBite frontend
2. Frontend sends credentials to `api_auth.php?action=login`
3. ServeSoft creates a PHP session
4. User data is stored in localStorage and session is maintained via cookies

### API Integration

The SmartBite frontend communicates with ServeSoft PHP endpoints:

- **Auth**: `/api_auth.php` - Login, register, logout, session verification
- **Customer**: `/api_customer.php` - Menu browsing, cart management, orders
- **Manager**: `/api_manager.php` - Restaurant management, menu items, order management
- **Driver**: `/api_driver.php` - Delivery management
- **Admin**: `/api_admin.php` - User and restaurant administration
- **Bootstrap**: `/bootstrap.php` - Initial data loading (restaurants, tables, menu)

### Role Mapping

ServeSoft roles map to SmartBite roles:

- `customer` → Customer (browse menu, place orders)
- `manager` → Restaurant Owner (manage restaurant and menu)
- `driver` → Delivery Agent (manage deliveries)
- `admin` → Admin (platform administration)

## Configuration Files

### `.env`
```
VITE_API_URL=http://localhost/github%20REpo/servesoft
CLIENT_URL=http://localhost:5173
```

### `vite.config.ts`
Configured with proxy to handle CORS and route API calls correctly.

### CORS Configuration
All PHP API files include CORS headers to allow requests from `http://localhost:5173`

## Testing the Integration

1. **Register a new user**: Go to `/register` and create an account
2. **Login**: Use your credentials at `/login`
3. **Browse restaurants**: View available restaurants from the database
4. **View menu**: See menu items from a specific restaurant
5. **Place order**: Add items to cart and checkout

## Troubleshooting

### CORS Errors
- Make sure Apache is running
- Verify CORS headers are in all PHP API files
- Check that the frontend is running on port 5173

### Session Issues
- PHP sessions use cookies
- Make sure `withCredentials: true` is set in axios
- Check that session is started in PHP files

### Database Connection
- Verify MySQL is running
- Check `config.php` for correct database credentials
- Ensure `SERVESOFT` database exists

## File Structure

```
project/
├── github REpo/
│   ├── smartbite/          # React frontend
│   │   ├── src/
│   │   │   ├── services/
│   │   │   │   └── api.js  # API integration layer
│   │   │   ├── contexts/
│   │   │   │   └── AuthContext.tsx
│   │   │   └── ...
│   │   ├── vite.config.ts
│   │   └── package.json
│   │
│   └── servesoft/          # PHP backend
│       ├── api_auth.php
│       ├── api_customer.php
│       ├── api_manager.php
│       ├── api_driver.php
│       ├── api_admin.php
│       ├── bootstrap.php
│       ├── config.php
│       └── ...
│
└── .env                    # Environment configuration
```

## Notes

- Socket.IO functionality has been removed (ServeSoft doesn't support WebSockets)
- Real-time features are not available in this integration
- Session-based authentication is used instead of JWT tokens
- All API calls go through the PHP backend, no direct database access from frontend
