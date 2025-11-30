# Vehicle Parking App V2

A comprehensive multi-user parking management application built with Flask backend and Vue.js frontend for the Modern Application Development II course at IIT Madras.

## 🚀 Features

- **Multi-user System**: Admin and User roles with JWT-based authentication
- **Parking Management**: Complete CRUD operations for parking lots and spots
- **Real-time Booking**: Users can book available parking spots with automatic allocation
- **Background Jobs**: 
  - Daily email reminders for active reservations
  - Monthly activity reports for admins
  - Async CSV export with email delivery
- **Performance**: Redis caching for optimized API responses
- **Analytics**: Interactive charts and statistics for both admin and users
- **Email Notifications**: Professional HTML email templates

## 📋 Prerequisites

- Python 3.8+
- Redis Server (Memurai for Windows)
- Node.js 16+ and npm
- Git

## 🛠️ Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd "MAD-II Project"
```

### 2. Backend Setup

```bash
# Create and activate virtual environment
python -m venv venv

# On Windows (PowerShell)
venv\Scripts\Activate.ps1

# On Linux/Mac
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Frontend Setup

```bash
cd frontend
npm install
```

### 4. Configure Environment Variables

Update the `.env` file in the project root:

```env
# Flask Configuration
FLASK_APP=run.py
FLASK_ENV=development
SECRET_KEY=your-secret-key

# Database
DATABASE_URL=sqlite:///C:/MAD-II Project/backend/parking.db

# Redis
REDIS_URL=redis://localhost:6379/0
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/0

# Email Configuration (Gmail)
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USE_TLS=True
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password
MAIL_DEFAULT_SENDER=your-email@gmail.com
```

**Note**: For Gmail, you need to generate an App Password from https://myaccount.google.com/apppasswords

### 5. Initialize Database

```bash
cd backend

# Create database tables and admin user
python init_db.py init

# Add sample parking lots (optional)
python init_db.py seed
```

**Default Admin Credentials:**
- Username: `admin`
- Password: `admin123`
- ⚠️ Change this password after first login!

### 6. Start Redis Server

**On Windows:**
```bash
# Install Memurai (Redis for Windows)
# Download from: https://www.memurai.com/

# Start Memurai service
# It runs automatically after installation
```

**On Linux/Mac:**
```bash
redis-server
```

## 🚀 Running the Application

You need to run **4 terminals** simultaneously:

### Terminal 1: Backend API
```bash
cd backend
python run.py
```
Backend runs on: http://localhost:5000

### Terminal 2: Frontend
```bash
cd frontend
npm run dev
```
Frontend runs on: http://localhost:5173

### Terminal 3: Celery Worker
```bash
cd backend
python -m celery -A celery_worker worker --loglevel=info --pool=solo
```

### Terminal 4: Celery Beat (Optional - for scheduled tasks)
```bash
cd backend
python -m celery -A celery_worker beat --loglevel=info
```

## 📁 Project Structure

```
MAD-II Project/
├── backend/
│   ├── app/
│   │   ├── __init__.py          # Flask app factory
│   │   ├── models.py            # Database models
│   │   ├── auth/                # Authentication routes
│   │   ├── admin/               # Admin routes
│   │   ├── user/                # User routes
│   │   ├── export/              # CSV export routes
│   │   ├── tasks/               # Celery tasks
│   │   └── utils/               # Utility functions (caching)
│   ├── config.py                # Configuration
│   ├── run.py                   # Application entry point
│   ├── celery_worker.py         # Celery worker
│   ├── init_db.py               # Database initialization
│   └── parking.db               # SQLite database
├── frontend/
│   ├── src/
│   │   ├── components/          # Vue components
│   │   ├── views/               # Page views
│   │   ├── stores/              # Pinia stores
│   │   ├── router/              # Vue Router
│   │   └── App.vue              # Root component
│   └── package.json             # Node dependencies
├── templates/
│   └── emails/                  # HTML email templates
├── .env                         # Environment variables
├── requirements.txt             # Python dependencies
├── PROJECT_REPORT.md            # Project documentation
└── README.md                    # This file
```

## 🗄️ Database Schema

### Core Tables
- **users**: User accounts with authentication
- **roles**: User roles (admin, user)
- **roles_users**: Many-to-many relationship for user roles
- **parking_lots**: Parking lot information
- **parking_spots**: Individual parking spots
- **reservations**: Booking records with vehicle information

See `PROJECT_REPORT.md` for the complete ER diagram.

## 🌐 API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User/Admin login
- `GET /api/auth/me` - Get current user
- `POST /api/auth/logout` - Logout

### Admin Endpoints
- `GET /api/admin/lots` - List all parking lots
- `POST /api/admin/lots` - Create parking lot
- `PUT /api/admin/lots/<id>` - Update parking lot
- `DELETE /api/admin/lots/<id>` - Delete parking lot
- `GET /api/admin/users` - List all users
- `GET /api/admin/analytics/overview` - System analytics

### User Endpoints
- `GET /api/lots` - Browse available parking lots
- `POST /api/reservations` - Book parking spot
- `PUT /api/reservations/<id>/checkout` - Release spot
- `GET /api/reservations/my` - View reservation history
- `GET /api/analytics/my` - View personal analytics

### Export Endpoints
- `POST /api/export/reservations` - Trigger CSV export (async)
- `GET /api/export/status/<task_id>` - Check export status

## 🧪 Testing

```bash
cd backend
python -m pytest tests/ -v
```

## 📦 Technologies Used

### Backend
| Component | Technology |
|-----------|-----------|
| Framework | Flask 2.3+ |
| Database | SQLite |
| ORM | SQLAlchemy 2.0+ |
| Authentication | Flask-JWT-Extended |
| Task Queue | Celery 5.3+ |
| Message Broker | Redis 7.0+ |
| Caching | Redis |
| Email | Flask-Mail |
| CORS | Flask-CORS |

### Frontend
| Component | Technology |
|-----------|-----------|
| Framework | Vue.js 3.3+ |
| Router | Vue Router 4.2+ |
| State Management | Pinia 2.1+ |
| UI Framework | Bootstrap 5.3+ |
| Charts | Chart.js 4.4+ |
| HTTP Client | Axios 1.6+ |
| Build Tool | Vite |

## 🔐 Security Notes

1. Change default admin password immediately
2. Update SECRET_KEY in production
3. Use environment variables for sensitive data
4. Never commit `.env` file to version control
5. Use Gmail App Passwords for email functionality

## 🆘 Troubleshooting

### Database Issues
```bash
# Reset database if you encounter errors
python backend/init_db.py reset
```

### Redis Connection Error
- Ensure Redis/Memurai server is running
- Check REDIS_URL in .env file
- Test connection: `redis-cli ping` (should return PONG)

### Email Not Sending
1. Verify Gmail App Password is correct (16 characters)
2. Check MAIL_USERNAME and MAIL_DEFAULT_SENDER match
3. Ensure Celery worker is running
4. Check Celery worker logs for errors

### Port Already in Use
```bash
# Backend - change port in backend/run.py
app.run(debug=True, host='0.0.0.0', port=5001)

# Frontend - change port in frontend/vite.config.js
server: { port: 5174 }
```

## 📝 Documentation

- **PROJECT_REPORT.md** - Complete project documentation
- **VIDEO_PRESENTATION_OUTLINE.md** - Video presentation guide

## 👤 Author

Student Project - Modern Application Development II  
IIT Madras

## 📄 License

Educational use only.

## 🎓 Course Information

**Course**: Modern Application Development II  
**Institution**: IIT Madras  
**Project**: Vehicle Parking Management Application
