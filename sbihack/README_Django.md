# SBI Event Tracking System - Django Full Stack Application

A complete Django-based web application for tracking SBI banking events with GPS location data and advanced analytics using Kalman-Cluster Fusion algorithm.

## Features

### 🏦 User Portal

- **Aadhaar-based Registration**: Uses 12-digit Aadhaar number as primary key
- **Real-time GPS Tracking**: Records precise location data for banking events
- **Three Event Types**:
  - SBI Login Events
  - UPI Transaction Events
  - SBI App Open Events
- **Interactive Dashboard**: View recorded events and location history

### 🛡️ Authority Portal

- **Administrative Dashboard**: Comprehensive data overview and analytics
- **Data Export**: Download all user events in JSON format
- **Advanced Analytics**: Process data using Kalman-Cluster Fusion algorithm
- **Visual Reports**: Detailed analysis results with clustering visualization

### 🧠 Kalman-Cluster Fusion Algorithm

- **DBSCAN Clustering**: Groups events by geographical proximity (eps=0.01°)
- **Event Weighting System**:
  - UPI Transactions: Weight 1.0
  - App Open: Weight 0.8
  - Login: Weight 0.6
- **Time Decay**: 72-hour decay factor for event relevance
- **Night Boost**: 1.2x multiplier for events between 22:00-06:00
- **Prediction System**: Generates location predictions based on clustered patterns

## Installation & Setup

### Prerequisites

- Python 3.9+
- Django 4.2.7
- Required packages (see requirements.txt)

### Quick Start

1. **Install Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

2. **Run Migrations**

   ```bash
   python manage.py migrate
   ```

3. **Start Development Server**

   ```bash
   python manage.py runserver
   ```

4. **Access Application**
   - Main Application: http://127.0.0.1:8000/
   - Admin Panel: http://127.0.0.1:8000/admin/

## User Credentials

### Super Admin (Authority)

- **Username**: `admin`
- **Password**: `sbi123`
- **Access**: Full authority dashboard and admin panel

### Regular Users

- Register with 12-digit Aadhaar number
- All users automatically marked as defaulters for tracking
- Access user dashboard for event recording

## Application Structure

```
sbi_project/
├── sbi_app/
│   ├── models.py          # Database models (SBIUser, UserEvent, etc.)
│   ├── views.py           # Application logic and API endpoints
│   ├── forms.py           # User registration and login forms
│   ├── utils.py           # Kalman-Cluster Fusion algorithm
│   ├── admin.py           # Django admin configuration
│   ├── urls.py            # URL routing
│   └── templates/         # HTML templates
├── sbi_project/
│   ├── settings.py        # Django configuration
│   └── urls.py            # Main URL configuration
├── manage.py              # Django management script
├── requirements.txt       # Python dependencies
└── create_admin.py        # Admin user creation script
```

## Key URLs

- `/` - Home page with login options
- `/register/` - User registration
- `/login/` - User login
- `/authority/login/` - Authority login
- `/dashboard/` - User dashboard
- `/authority/` - Authority dashboard
- `/authority/download/` - Download data (JSON)
- `/authority/process/` - Process data with algorithm
- `/authority/analyses/` - View all analyses

## Database Models

### SBIUser

- Custom user model with Aadhaar as primary key
- Fields: aadhaar_number, first_name, last_name, email, is_authority, is_defaulter

### UserEvent

- Stores banking events with GPS coordinates
- Fields: user, event_type, timestamp, latitude, longitude, accuracy

### ProcessedData

- Stores analysis results from Kalman-Cluster algorithm
- Fields: processed_at, total_events, total_users, analysis_results

### EventWeight

- Configurable weights for different event types
- Fields: event_type, weight, description

## Security Features

- **CSRF Protection**: All forms protected against CSRF attacks
- **User Authentication**: Session-based authentication system
- **Role-based Access**: Separate user and authority access levels
- **Data Validation**: Input validation for Aadhaar numbers and coordinates

## API Endpoints

### POST /record-event/

Records a new banking event with GPS coordinates

```json
{
  "event_type": "login|upi|app_open",
  "latitude": 26.1234,
  "longitude": 91.5678,
  "accuracy": 15.0
}
```

### GET /authority/download/

Downloads all events in JSON format (Authority only)

### POST /authority/process/

Processes events using Kalman-Cluster Fusion algorithm (Authority only)

## Algorithm Details

The Kalman-Cluster Fusion algorithm processes user events through these steps:

1. **Data Collection**: Gather all user events with timestamps and coordinates
2. **Clustering**: Use DBSCAN to group events by location (eps=0.01°, min_samples=2)
3. **Weight Calculation**: Apply event weights with time decay and night boost
4. **Prediction Generation**: Calculate weighted centroids for each cluster
5. **Analysis Output**: Generate comprehensive reports with predictions

## Browser Compatibility

- **Chrome/Edge**: Full GPS support
- **Firefox**: Full GPS support
- **Safari**: GPS support with user permission
- **Mobile Browsers**: GPS support on mobile devices

## Production Deployment

For production deployment:

1. Set `DEBUG = False` in settings.py
2. Configure proper database (PostgreSQL recommended)
3. Set up static file serving
4. Configure HTTPS for location services
5. Set proper ALLOWED_HOSTS

## Hackathon Ready

This application is designed for hackathon environments with:

- ✅ Complete working features
- ✅ Real GPS integration
- ✅ Advanced ML algorithm
- ✅ Professional UI/UX
- ✅ Comprehensive documentation
- ✅ Easy setup and demonstration

## Demo Flow

1. **Registration**: Create account with Aadhaar number
2. **Login**: Access user dashboard
3. **Record Events**: Click buttons to record banking events with GPS
4. **Authority Login**: Use admin credentials
5. **View Data**: See all user events in authority dashboard
6. **Download**: Export data as JSON
7. **Process**: Run Kalman-Cluster analysis
8. **Results**: View detailed analysis with clusters and predictions

## Support

For issues or questions:

- Check Django logs in terminal
- Verify GPS permissions in browser
- Ensure all migrations are applied
- Check superuser creation was successful

---

**Note**: This system is designed for demonstration purposes in a hackathon environment. All users are automatically marked as defaulters for tracking and analysis purposes.
