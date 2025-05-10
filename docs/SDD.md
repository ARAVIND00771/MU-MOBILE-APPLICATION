# Software Design Document (SDD)

## 1. System Architecture

### 1.1 High-Level Architecture
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Mobile    │     │   Backend   │     │  Firebase   │
│    App      │◄───►│   Server    │◄───►│  Services   │
└─────────────┘     └─────────────┘     └─────────────┘
      ▲                    ▲                    ▲
      │                    │                    │
      ▼                    ▼                    ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Offline   │     │   Redis     │     │  Cloud      │
│   Storage   │     │   Cache     │     │  Storage    │
└─────────────┘     └─────────────┘     └─────────────┘
```

### 1.2 Component Diagram
- **Frontend Layer**
  - React Native Components (v0.72.0)
  - Navigation System (React Navigation 6)
  - State Management (Redux Toolkit)
  - UI Components (React Native Paper)

- **Backend Layer**
  - Express.js Server (v4.18.2)
  - API Routes (RESTful)
  - Middleware (Authentication, Validation)
  - Services (Business Logic)

- **Database Layer**
  - Firestore Collections
  - Real-time Listeners
  - Data Models
  - Indexes

## 2. Detailed Design

### 2.1 Frontend Design
#### 2.1.1 Component Structure
```
src/
├── components/
│   ├── auth/
│   │   ├── Login.tsx
│   │   ├── Register.tsx
│   │   └── ForgotPassword.tsx
│   ├── location/
│   │   ├── LocationTracker.tsx
│   │   ├── GeofenceMap.tsx
│   │   └── EmergencyButton.tsx
│   ├── announcements/
│   │   ├── AnnouncementList.tsx
│   │   ├── AnnouncementDetail.tsx
│   │   └── CreateAnnouncement.tsx
│   └── common/
│       ├── Header.tsx
│       ├── Footer.tsx
│       └── LoadingSpinner.tsx
├── screens/
│   ├── HomeScreen.tsx
│   ├── ProfileScreen.tsx
│   └── SettingsScreen.tsx
├── navigation/
│   ├── AppNavigator.tsx
│   └── AuthNavigator.tsx
├── services/
│   ├── api.ts
│   ├── auth.ts
│   └── location.ts
└── utils/
    ├── constants.ts
    ├── helpers.ts
    └── types.ts
```

#### 2.1.2 State Management
```typescript
// Redux Store Configuration
import { configureStore } from '@reduxjs/toolkit';
import authReducer from './slices/authSlice';
import locationReducer from './slices/locationSlice';

export const store = configureStore({
  reducer: {
    auth: authReducer,
    location: locationReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false,
    }),
});

// Example Slice
import { createSlice } from '@reduxjs/toolkit';

const locationSlice = createSlice({
  name: 'location',
  initialState: {
    currentLocation: null,
    geofences: [],
    isTracking: false,
  },
  reducers: {
    setLocation: (state, action) => {
      state.currentLocation = action.payload;
    },
    // ... other reducers
  },
});
```

### 2.2 Backend Design
#### 2.2.1 API Structure
```
/api
├── auth/
│   ├── login
│   ├── register
│   └── refresh-token
├── users/
│   ├── profile
│   ├── settings
│   └── permissions
├── location/
│   ├── track
│   ├── geofence
│   └── history
├── announcements/
│   ├── create
│   ├── list
│   └── read-receipts
└── university/
    ├── courses
    ├── events
    └── departments
```

#### 2.2.2 Service Layer
```typescript
// User Service Example
class UserService {
  async createUser(userData: UserInput): Promise<User> {
    try {
      const userRef = await admin.auth().createUser({
        email: userData.email,
        password: userData.password,
      });

      await db.collection('users').doc(userRef.uid).set({
        ...userData,
        createdAt: admin.firestore.FieldValue.serverTimestamp(),
      });

      return this.getUser(userRef.uid);
    } catch (error) {
      throw new Error(`Failed to create user: ${error.message}`);
    }
  }

  // ... other methods
}

// Location Service Example
class LocationService {
  async trackLocation(userId: string, location: LocationData): Promise<void> {
    const batch = db.batch();
    
    // Update current location
    batch.set(
      db.collection('locations').doc(userId),
      {
        ...location,
        updatedAt: admin.firestore.FieldValue.serverTimestamp(),
      }
    );

    // Add to history
    batch.set(
      db.collection('locationHistory').doc(),
      {
        userId,
        ...location,
        timestamp: admin.firestore.FieldValue.serverTimestamp(),
      }
    );

    await batch.commit();
  }
}
```

### 2.3 Database Design
#### 2.3.1 Collections
```typescript
// Firestore Collections Structure
interface Collections {
  users: {
    [userId: string]: {
      email: string;
      role: 'student' | 'parent' | 'faculty' | 'admin';
      profile: {
        name: string;
        department?: string;
        studentId?: string;
        parentOf?: string[];
      };
      settings: {
        notifications: boolean;
        locationSharing: boolean;
      };
      createdAt: Timestamp;
      updatedAt: Timestamp;
    };
  };
  
  locations: {
    [userId: string]: {
      coordinates: {
        latitude: number;
        longitude: number;
      };
      accuracy: number;
      timestamp: Timestamp;
    };
  };
  
  announcements: {
    [announcementId: string]: {
      title: string;
      content: string;
      author: string;
      category: string;
      priority: 'low' | 'medium' | 'high';
      targetAudience: string[];
      attachments: string[];
      createdAt: Timestamp;
      expiresAt?: Timestamp;
    };
  };
}
```

#### 2.3.2 Data Models
```typescript
// TypeScript Interfaces
interface User {
  id: string;
  email: string;
  role: 'student' | 'parent' | 'faculty' | 'admin';
  profile: {
    name: string;
    department?: string;
    studentId?: string;
    parentOf?: string[];
  };
  settings: {
    notifications: boolean;
    locationSharing: boolean;
  };
  createdAt: Date;
  updatedAt: Date;
}

interface Location {
  userId: string;
  coordinates: {
    latitude: number;
    longitude: number;
  };
  accuracy: number;
  timestamp: Date;
  geofenceId?: string;
}

interface Announcement {
  id: string;
  title: string;
  content: string;
  author: string;
  category: 'academic' | 'event' | 'emergency' | 'general';
  priority: 'low' | 'medium' | 'high';
  targetAudience: string[];
  attachments: string[];
  readReceipts: {
    [userId: string]: {
      readAt: Date;
      deviceInfo: string;
    };
  };
  createdAt: Date;
  expiresAt?: Date;
}
```

## 3. Security Design

### 3.1 Authentication
```typescript
// JWT Middleware
const authenticateToken = async (req: Request, res: Response, next: NextFunction) => {
  try {
    const token = req.headers.authorization?.split(' ')[1];
    if (!token) {
      throw new Error('No token provided');
    }

    const decoded = await admin.auth().verifyIdToken(token);
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

// Role-based Access Control
const checkRole = (roles: string[]) => {
  return (req: Request, res: Response, next: NextFunction) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    next();
  };
};
```

### 3.2 Data Protection
- End-to-end encryption using AES-256
- Input validation using Joi
- XSS prevention using helmet
- Rate limiting using express-rate-limit

## 4. Performance Design

### 4.1 Optimization Strategies
```typescript
// Caching Example
const cacheMiddleware = (duration: number) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    const key = `cache:${req.originalUrl}`;
    const cachedResponse = await redis.get(key);
    
    if (cachedResponse) {
      return res.json(JSON.parse(cachedResponse));
    }
    
    res.sendResponse = res.json;
    res.json = (body: any) => {
      redis.setex(key, duration, JSON.stringify(body));
      return res.sendResponse(body);
    };
    
    next();
  };
};

// Pagination Example
const paginateResults = (page: number, limit: number) => {
  const skip = (page - 1) * limit;
  return {
    skip,
    limit,
  };
};
```

### 4.2 Scalability
- Horizontal scaling with load balancers
- Database indexing for common queries
- Query optimization with compound indexes
- Caching frequently accessed data

## 5. Testing Strategy

### 5.1 Unit Testing
```typescript
// Jest Test Example
describe('UserService', () => {
  it('should create a new user', async () => {
    const userData = {
      email: 'test@example.com',
      password: 'password123',
      role: 'student',
    };
    
    const user = await userService.createUser(userData);
    expect(user.email).toBe(userData.email);
    expect(user.role).toBe(userData.role);
  });
});
```

### 5.2 Integration Testing
- API endpoint testing
- Database operations testing
- Authentication flow testing
- Real-time updates testing

### 5.3 End-to-End Testing
- User flow testing
- Cross-platform testing
- Performance testing
- Security testing

## 6. Deployment Strategy

### 6.1 Frontend Deployment
- Expo build system
- App store submission
- Play store submission
- CI/CD pipeline

### 6.2 Backend Deployment
- Node.js hosting
- Environment configuration
- SSL/TLS setup
- Monitoring and logging

### 6.3 Database Deployment
- Firebase project setup
- Security rules
- Backup configuration
- Monitoring and alerts 