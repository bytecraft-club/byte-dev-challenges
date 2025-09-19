# 🌐 Frontend Dev Challenge: Prayer Times Web Application

## Challenge Overview
Create a responsive web application that displays Islamic prayer times using the Aladhan Prayer Times API. This challenge focuses on modern frontend development practices, responsive design, and creating an engaging user experience that can be completed in **one day**.

## 🎯 Objectives
- Build a modern, responsive web interface
- Implement advanced API integration with error handling
- Create interactive features and smooth animations
- Optimize for performance and accessibility
- Deploy a production-ready application

## 🛠 Technical Requirements

### Core Features (Must Have)
1. **Responsive Design**: Perfect display on desktop, tablet, and mobile
2. **Location Services**: Geolocation API integration with fallback options
3. **Prayer Times Dashboard**: Interactive display with visual enhancements
4. **Search Functionality**: City/country search with autocomplete
5. **Progressive Web App**: Offline capabilities and installable

### Advanced Features (Challenge Level)
1. **Prayer Time Visualization**: Interactive charts showing prayer intervals
2. **Multiple Calculation Methods**: Different Islamic calculation methods
3. **Prayer Time History**: Calendar view of past/future prayer times
4. **Customizable Themes**: Multiple color schemes and layouts
5. **Prayer Reminders**: Browser notifications with custom sounds
6. **Qibla Compass**: Visual direction indicator to Mecca
7. **Islamic Calendar Integration**: Hijri calendar with Islamic events

## 🔧 Technology Stack Options

### React + Modern Tools
```bash
npx create-react-app prayer-times-web
cd prayer-times-web
npm install axios styled-components framer-motion
npm install @mui/material @emotion/react @emotion/styled
npm install react-query workbox-webpack-plugin
```

### Vue.js + Composition API
```bash
npm create vue@latest prayer-times-web
cd prayer-times-web
npm install axios @vueuse/core
npm install vuetify @mdi/js
npm install @tanstack/vue-query
```

### Angular + Material
```bash
ng new prayer-times-web
cd prayer-times-web
ng add @angular/material
ng add @angular/pwa
npm install @angular/google-maps
```

### Vanilla JavaScript + Modern Features
```bash
npm init -y
npm install vite axios chart.js
npm install workbox-window leaflet
```

## 📡 Advanced API Integration

### Base URL & Authentication
```javascript
const API_BASE = 'https://api.aladhan.com/v1';
const BACKUP_API = 'https://api.aladhan.com/v1'; // Same but for redundancy

// API Client with error handling
class PrayerTimesAPI {
  constructor() {
    this.cache = new Map();
    this.retryCount = 3;
  }

  async fetchWithRetry(url, options = {}) {
    for (let i = 0; i < this.retryCount; i++) {
      try {
        const response = await fetch(url, options);
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        return await response.json();
      } catch (error) {
        if (i === this.retryCount - 1) throw error;
        await this.delay(1000 * Math.pow(2, i)); // Exponential backoff
      }
    }
  }

  delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}
```

### Enhanced Endpoints
1. **Prayer Times by Coordinates (with method selection)**
   ```
   GET /timings/{date}?latitude={lat}&longitude={lng}&method={method}&tune={tune}
   ```

2. **Monthly Prayer Times**
   ```
   GET /calendar/{year}/{month}?latitude={lat}&longitude={lng}&method={method}
   ```

3. **Calculation Methods**
   ```
   GET /methods
   ```

4. **Islamic Calendar Events**
   ```
   GET /specialDays/{year}
   ```

## 🎨 Advanced UI/UX Design

### Modern Design System
```css
:root {
  /* Color Palette */
  --primary-50: #f0f9f4;
  --primary-500: #10b981;
  --primary-900: #064e3b;
  
  /* Typography Scale */
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-3xl: 1.875rem;
  
  /* Spacing Scale */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-8: 2rem;
  --space-16: 4rem;
  
  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1);
}
```

### Component Architecture
```
src/
├── components/
│   ├── common/
│   │   ├── Loading.jsx
│   │   ├── ErrorBoundary.jsx
│   │   └── Notification.jsx
│   ├── prayer/
│   │   ├── PrayerCard.jsx
│   │   ├── PrayerTimeline.jsx
│   │   ├── NextPrayerCountdown.jsx
│   │   └── PrayerHistory.jsx
│   ├── location/
│   │   ├── LocationPicker.jsx
│   │   ├── CitySearch.jsx
│   │   └── QiblaCompass.jsx
│   └── layout/
│       ├── Header.jsx
│       ├── Sidebar.jsx
│       └── Footer.jsx
├── hooks/
│   ├── useGeolocation.js
│   ├── usePrayerTimes.js
│   └── useNotifications.js
├── services/
│   ├── api.js
│   ├── storage.js
│   └── notifications.js
└── utils/
    ├── calculations.js
    ├── formatters.js
    └── constants.js
```

## 🚀 Implementation Roadmap

### Phase 1: Foundation (1 hour)
1. **Project Setup**
   ```bash
   # Create optimized build setup
   npm install --save-dev webpack-bundle-analyzer
   npm install --save-dev lighthouse
   ```

2. **Core Architecture**
   - API service layer with caching
   - State management setup
   - Routing configuration
   - Error boundary implementation

### Phase 2: Core Features (2.5 hours)
1. **Location Integration**
   ```javascript
   // Advanced geolocation with high accuracy
   const useGeolocation = () => {
     const [location, setLocation] = useState(null);
     const [error, setError] = useState(null);
     
     useEffect(() => {
       if (!navigator.geolocation) {
         setError('Geolocation not supported');
         return;
       }
       
       navigator.geolocation.getCurrentPosition(
         (position) => {
           setLocation({
             latitude: position.coords.latitude,
             longitude: position.coords.longitude,
             accuracy: position.coords.accuracy
           });
         },
         (error) => setError(error.message),
         {
           enableHighAccuracy: true,
           timeout: 10000,
           maximumAge: 300000 // 5 minutes cache
         }
       );
     }, []);
     
     return { location, error };
   };
   ```

2. **Prayer Times Dashboard**
   - Animated prayer cards
   - Real-time countdown timers
   - Visual prayer timeline
   - Interactive time selection

### Phase 3: Advanced Features (2.5 hours)
1. **Prayer Visualization**
   ```javascript
   // Chart.js integration for prayer intervals
   import {
     Chart as ChartJS,
     CategoryScale,
     LinearScale,
     PointElement,
     LineElement,
     Title,
     Tooltip,
     Legend,
   } from 'chart.js';

   const PrayerChart = ({ prayerTimes }) => {
     const data = {
       labels: Object.keys(prayerTimes),
       datasets: [{
         label: 'Prayer Times',
         data: Object.values(prayerTimes).map(timeToMinutes),
         borderColor: 'rgb(16, 185, 129)',
         backgroundColor: 'rgba(16, 185, 129, 0.2)',
         tension: 0.4
       }]
     };
     
     return <Line data={data} options={chartOptions} />;
   };
   ```

2. **PWA Implementation**
   ```javascript
   // Service Worker registration
   if ('serviceWorker' in navigator) {
     window.addEventListener('load', () => {
       navigator.serviceWorker.register('/sw.js')
         .then(registration => {
           console.log('SW registered: ', registration);
         })
         .catch(registrationError => {
           console.log('SW registration failed: ', registrationError);
         });
     });
   }
   ```

### Phase 4: Enhanced UX (1.5 hours)
1. **Animations & Transitions**
   ```javascript
   // Framer Motion animations
   import { motion, AnimatePresence } from 'framer-motion';
   
   const PrayerCard = ({ prayer, time, isNext }) => (
     <motion.div
       initial={{ opacity: 0, y: 20 }}
       animate={{ opacity: 1, y: 0 }}
       exit={{ opacity: 0, y: -20 }}
       whileHover={{ scale: 1.02 }}
       className={`prayer-card ${isNext ? 'next-prayer' : ''}`}
     >
       <h3>{prayer}</h3>
       <time>{time}</time>
     </motion.div>
   );
   ```

2. **Responsive Design**
   ```css
   .prayer-grid {
     display: grid;
     grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
     gap: 1.5rem;
     
     @media (max-width: 768px) {
       grid-template-columns: 1fr;
       gap: 1rem;
     }
   }
   ```

### Phase 5: Testing & Optimization (30 minutes)
1. **Performance Optimization**
   - Code splitting
   - Image optimization
   - Bundle analysis
   - Lighthouse audit

2. **Accessibility Testing**
   - ARIA labels
   - Keyboard navigation
   - Color contrast
   - Screen reader testing

## 🧪 Advanced Testing Strategy

### Unit Tests
```javascript
// Prayer times calculation tests
describe('Prayer Times Utils', () => {
  test('should calculate correct prayer intervals', () => {
    const prayerTimes = {
      Fajr: '05:30',
      Dhuhr: '12:30',
      Asr: '15:45',
      Maghrib: '18:15',
      Isha: '19:45'
    };
    
    const intervals = calculatePrayerIntervals(prayerTimes);
    expect(intervals.fajrToDhuhr).toBe(420); // 7 hours in minutes
  });
});
```

### Integration Tests
```javascript
// API integration tests
describe('Prayer Times API', () => {
  test('should fetch prayer times for valid coordinates', async () => {
    const api = new PrayerTimesAPI();
    const result = await api.getPrayerTimes(40.7128, -74.0060);
    
    expect(result).toHaveProperty('data.timings');
    expect(result.data.timings).toHaveProperty('Fajr');
  });
});
```

### E2E Tests (Cypress)
```javascript
describe('Prayer Times App', () => {
  it('should display prayer times after location access', () => {
    cy.visit('/');
    cy.get('[data-testid="allow-location"]').click();
    cy.get('[data-testid="prayer-times"]').should('be.visible');
    cy.get('[data-testid="prayer-card"]').should('have.length', 5);
  });
});
```

## 🎖 Challenge Levels

### Beginner (4-5 hours)
- Basic responsive layout
- Simple API integration
- Static prayer times display

### Intermediate (6-7 hours)
- Interactive features
- PWA capabilities
- Custom animations
- Advanced error handling

### Advanced (8+ hours)
- Complex visualizations
- Multiple calculation methods
- Offline functionality
- Performance optimization
- Comprehensive testing

## 📊 Performance Benchmarks
Your app should achieve:
- **Lighthouse Score**: 90+ across all categories
- **First Contentful Paint**: < 2s
- **Largest Contentful Paint**: < 3s
- **Cumulative Layout Shift**: < 0.1
- **Bundle Size**: < 500KB initial load

## 🚀 Deployment Options
1. **Vercel**: `npx vercel --prod`
2. **Netlify**: `npm run build && netlify deploy`
3. **GitHub Pages**: Automated via GitHub Actions
4. **Firebase Hosting**: `firebase deploy`

## 📝 Submission Requirements
1. **Live Demo**: Deployed application URL
2. **Source Code**: GitHub repository with clear README
3. **Documentation**: API integration and architecture decisions
4. **Performance Report**: Lighthouse audit results
5. **Demo Video**: 2-3 minute walkthrough
6. **Responsive Screenshots**: Desktop, tablet, mobile views

## 🏆 Success Criteria
Your web application should:
- Load prayer times within 3 seconds
- Work perfectly across all device sizes
- Handle offline scenarios gracefully
- Achieve 90+ Lighthouse performance score
- Demonstrate smooth animations and interactions
- Be production-ready and deployable

**Ready to build an exceptional prayer times web experience? Start coding! 🕌✨**
