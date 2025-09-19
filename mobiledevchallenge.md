# 📱 Mobile Dev Challenge: Prayer Times App

## Challenge Overview
Build a simple mobile application that displays daily prayer times for any location using the Aladhan Prayer Times API. This challenge is designed to be completed in **one day** and focuses on API integration, basic UI design, and location services.

## 🎯 Objectives
- Create a clean, intuitive mobile interface
- Integrate with the Aladhan Prayer Times API
- Implement location-based prayer times
- Display prayer times in an organized format
- Add basic Islamic calendar information

## 🛠 Technical Requirements

### Core Features (Must Have)
1. **Location Detection**: Get user's current location or allow manual city input
2. **Prayer Times Display**: Show today's 5 prayer times (Fajr, Dhuhr, Asr, Maghrib, Isha)
3. **Date Information**: Display current Hijri and Gregorian dates
4. **Responsive Design**: Works on both phones and tablets

### Bonus Features (Nice to Have)
1. **Next Prayer Countdown**: Show time remaining until next prayer
2. **Qibla Direction**: Display compass pointing to Mecca
3. **Multiple Cities**: Save and switch between multiple locations
4. **Prayer Notifications**: Local notifications for prayer times
5. **Theme Options**: Light/dark mode toggle

## 🔧 Technology Stack Options

### React Native
```bash
npx react-native init PrayerTimesApp
cd PrayerTimesApp
npm install @react-native-community/geolocation axios react-native-vector-icons
```

### Flutter
```bash
flutter create prayer_times_app
cd prayer_times_app
flutter pub add http geolocator permission_handler
```

### Native Android (Kotlin)
- Retrofit for API calls
- LocationManager for GPS
- RecyclerView for prayer list

### Native iOS (Swift)
- URLSession for networking
- Core Location for GPS
- UITableView for prayer list

## 📡 API Integration

### Base URL
```
https://api.aladhan.com/v1
```

### Key Endpoints
1. **Get Prayer Times by City**
   ```
   GET /timingsByCity/{date}?city={city}&country={country}&method=2
   ```

2. **Get Prayer Times by Coordinates**
   ```
   GET /timings/{date}?latitude={lat}&longitude={lng}&method=2
   ```

3. **Get Islamic Calendar**
   ```
   GET /gToH/{date}
   ```

### Example Response
```json
{
  "code": 200,
  "status": "OK",
  "data": {
    "timings": {
      "Fajr": "05:30",
      "Sunrise": "07:00",
      "Dhuhr": "12:30",
      "Asr": "15:45",
      "Maghrib": "18:15",
      "Isha": "19:45"
    },
    "date": {
      "readable": "15 Sep 2024",
      "hijri": {
        "date": "10-03-1446",
        "format": "DD-MM-YYYY",
        "day": "10",
        "month": {
          "en": "Rabi' al-awwal"
        }
      }
    }
  }
}
```

## 🎨 UI/UX Guidelines

### Design Principles
- **Simplicity**: Clean, minimal interface focusing on essential information
- **Readability**: Large, clear fonts for prayer times
- **Cultural Sensitivity**: Use appropriate Islamic colors and typography
- **Accessibility**: Support for different screen sizes and accessibility features

### Suggested Color Palette
- Primary: #2E7D5A (Islamic Green)
- Secondary: #F4F4F4 (Light Gray)
- Accent: #D4AF37 (Gold)
- Text: #2C3E50 (Dark Gray)
- Background: #FFFFFF (White)

### Layout Structure
```
┌─────────────────────────────┐
│        Header               │
│    📍 Location              │
├─────────────────────────────┤
│    📅 Date Information      │
│    Hijri | Gregorian        │
├─────────────────────────────┤
│    🕌 Prayer Times          │
│    Fajr    05:30            │
│    Dhuhr   12:30            │
│    Asr     15:45            │
│    Maghrib 18:15            │
│    Isha    19:45            │
├─────────────────────────────┤
│    ⏰ Next Prayer: Asr      │
│    in 2 hours 15 minutes    │
└─────────────────────────────┘
```

## 🚀 Implementation Steps

### Phase 1: Setup (30 minutes)
1. Initialize project with chosen framework
2. Install required dependencies
3. Set up basic project structure
4. Configure development environment

### Phase 2: API Integration (2 hours)
1. Create API service class
2. Implement location detection
3. Fetch prayer times from API
4. Handle API errors and loading states
5. Test with different locations

### Phase 3: UI Development (3 hours)
1. Create main prayer times screen
2. Design prayer time cards/list items
3. Add location display
4. Implement date information display
5. Style according to design guidelines

### Phase 4: Enhanced Features (2 hours)
1. Add next prayer countdown
2. Implement location search
3. Add prayer time notifications (if time permits)
4. Test on different devices

### Phase 5: Testing & Polish (30 minutes)
1. Test with various locations
2. Handle edge cases (no internet, location denied)
3. Final UI adjustments
4. Add app icon and splash screen

## 🧪 Testing Checklist
- [ ] App launches successfully
- [ ] Location permission requested and handled
- [ ] Prayer times load for current location
- [ ] Manual location search works
- [ ] UI displays correctly on different screen sizes
- [ ] Offline state handled gracefully
- [ ] Prayer times update daily
- [ ] Next prayer countdown works (if implemented)

## 📚 Learning Resources
- [Aladhan API Documentation](https://aladhan.com/prayer-times-api)
- [Islamic Prayer Times Calculation Methods](https://aladhan.com/calculation-methods)
- [React Native Geolocation](https://github.com/michalchudziak/react-native-geolocation)
- [Flutter Geolocator Plugin](https://pub.dev/packages/geolocator)

## 🎖 Challenge Levels

### Beginner
- Use hardcoded location
- Display basic prayer times
- Simple list layout

### Intermediate  
- Add location detection
- Implement next prayer countdown
- Custom styled components

### Advanced
- Multiple city support
- Prayer notifications
- Qibla direction compass
- Offline caching

## 📝 Submission Guidelines
1. Upload source code to GitHub
2. Include screenshots in README
3. Document any challenges faced
4. Share APK/IPA file (optional)
5. Record a short demo video

## 🏆 Success Criteria
Your app should:
- Successfully fetch and display prayer times
- Have a clean, user-friendly interface
- Handle basic error states
- Work on the target mobile platform
- Be completed within 8 hours of focused development

**Ready to build? Start coding and may your app bring benefit to the Muslim community! 🕌**
