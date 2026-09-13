# LumpiScan – AI-Based Lumpy Skin Disease Detection System

## 📋 Project Overview

**LumpiScan** is a comprehensive, AI-powered web application designed to detect **Lumpy Skin Disease (LSD)** in cattle through image analysis. The platform combines deep learning computer vision with a veterinary network to provide farmers with real-time disease diagnostics and expert healthcare guidance.

This project bridges the gap between remote farmers and veterinary expertise, enabling **early detection** of contagious diseases, reducing disease spread, and facilitating faster treatment.

---

## 🎯 Project Motivation & Impact

### The Problem
Lumpy Skin Disease is a **highly contagious viral disease** affecting cattle, causing:
- Severe economic losses for farmers
- Rapid spread across herds if not detected early
- Limited access to veterinary care in rural areas
- Delayed diagnosis leading to disease progression

### The Solution
LumpiScan addresses these challenges by:

1. **Real-time AI Diagnostics**: Farmers can upload cattle images and receive instant AI-powered disease detection (~95% accuracy)
2. **Accessibility**: Works on mobile/web browsers without requiring physical veterinary visits
3. **Professional Network Integration**: Direct connection to registered veterinarians for consultation
4. **Digital Health Records**: Complete prediction history for tracking animal health over time
5. **Multi-language Support**: Available in English & Hindi for farmer accessibility

### Impact
- ✅ **Early Detection**: Identify LSD within minutes instead of days
- ✅ **Disease Control**: Isolate infected animals before herd-wide spread
- ✅ **Cost Reduction**: Prevent economic losses through preventive measures
- ✅ **Remote Care**: Connect rural farmers with expert veterinarians instantly
- ✅ **Data-Driven**: Digital records enable better herd management

---

## 🏗️ Architecture Overview

LumpiScan is built using a **three-tier architecture**:

```
┌─────────────────────────────────────────────────────────┐
│               FRONTEND (React + Vite)                   │
│  • Detection Interface  • Vet Search  • History Logs    │
│  • Multi-language Support (EN/HI)  • Dark Mode         │
│  • Authentication & User Management                    │
└────────────────┬────────────────────────────────────────┘
                 │ REST API (HTTP/CORS)
┌────────────────▼────────────────────────────────────────┐
│            BACKEND (Flask API - Port 5000)              │
│  • Image Upload & Preprocessing                         │
│  • ML Model Inference                                   │
│  • User & Veterinarian Management                       │
│  • Prediction History Storage                           │
│  • Veterinarian Search & Geo-location                   │
└────────────────┬────────────────────────────────────────┘
                 │
    ┌────────────┼────────────┐
    │            │            │
┌───▼───┐   ┌────▼────┐  ┌────▼────┐
│ML Model│   │Database │  │Uploads  │
│(Keras) │   │(JSON)   │  │(Images) │
└────────┘   └─────────┘  └─────────┘
```

### Component Details

#### Frontend (React + Vite)
- **Framework**: React 18.3 with React Router v6
- **Build Tool**: Vite for fast development and optimized builds
- **Styling**: Tailwind CSS with PostCSS
- **Features**:
  - Home landing page with feature showcase
  - Image detection interface with drag-and-drop
  - Veterinarian search & directory
  - Prediction history timeline
  - User authentication (phone-based)
  - Vet registration portal
  - Dark mode support
  - Responsive design (mobile-first)
  - Multi-language (English/Hindi) with i18n

#### Backend (Flask)
- **Framework**: Flask 3.0.3 with CORS support
- **Model Loading**: TensorFlow/Keras with fallback mechanisms
- **Image Processing**: Pillow for image manipulation
- **Database**: JSON-based persistent storage (database.json)
- **Server**: Gunicorn for production deployment
- **Port**: 5000 (development) / 10000 (production via Docker)

**Key Endpoints**:
- `GET /` – Health check
- `POST /predict` – Image upload and disease prediction
- `POST /register-user` – Register cattle owner
- `POST /login-user` – Login for cattle owners
- `POST /register-vet` – Register veterinarian
- `GET /search-doctors` – Find veterinarians by location
- `GET /history/<user_id>` – Fetch prediction history

#### ML Model (CNN - MobileNetV2)
- **Architecture**: MobileNetV2 (Keras)
- **Input Size**: 224×224 RGB images
- **Classes**: 2 (Healthy / Lumpy Skin Disease)
- **Accuracy**: ~95.15%
- **Inference Time**: <3 seconds
- **Model File**: `backend/model/lsd_final.keras`

---

## 🎨 Features

### 1. **AI Disease Detection**
- Upload cattle image (JPG/PNG)
- Real-time inference using MobileNetV2 CNN
- Confidence score display
- Health recommendations based on prediction
- Case ID for tracking

### 2. **Veterinary Network**
- Search vets by name, clinic, or location
- GPS-based distance calculation (Haversine formula)
- Vet ratings and reviews
- Direct phone contact
- Appointment booking interface
- Veterinarian registration portal

### 3. **Digital Health Records**
- Complete prediction history per user
- Timestamp tracking
- Confidence metrics
- Disease status tracking
- Re-examine previous cases

### 4. **User Management**
- Phone-based registration & authentication
- Cattle owner profiles
- Veterinarian profiles
- Location-based services

### 5. **User Experience**
- 🌙 Dark mode support
- 🌍 Multi-language (English/Hindi)
- 📱 Fully responsive design
- ⚡ Real-time feedback
- 🎨 Modern UI with Tailwind CSS
- ✨ Smooth animations & transitions

---

## 🛠️ Tech Stack

### Frontend
- **React** 18.3.1 – UI framework
- **React Router** 6.23.1 – Client-side routing
- **Tailwind CSS** 3.4.19 – Utility-first styling
- **Vite** 5.2.12 – Build tool
- **PostCSS** 8.5.10 – CSS processing

### Backend
- **Flask** 3.0.3 – Web framework
- **Flask-CORS** 4.0.1 – Cross-origin requests
- **TensorFlow** 2.21.0 – ML framework
- **Keras** 3.14.0 – Neural network API
- **Pillow** 10.3.0 – Image processing
- **Gunicorn** 22.0.0 – WSGI server

### ML/Data Science
- **Python** 3.11
- **TensorFlow/Keras** – Model training & inference
- **NumPy** – Numerical computing
- **Pillow** – Image manipulation

### DevOps & Deployment
- **Docker** – Containerization
- **Docker Compose** (optional) – Multi-container orchestration
- **Python venv** – Virtual environment

---

## 🧠 ML Model Architecture

### MobileNetV2-Based CNN

**Why MobileNetV2?**
- Lightweight (~3-4MB) – suitable for mobile deployment
- Fast inference (<3 seconds) – real-time predictions
- Transfer learning friendly – trained on ImageNet weights
- High accuracy – state-of-the-art for image classification

### Model Pipeline

```
Input Image (JPG/PNG)
     ↓
Resize to 224×224 (RGB)
     ↓
Normalize using MobileNetV2 preprocess_input
     ↓
MobileNetV2 Base (pretrained ImageNet weights)
     ↓
Custom Classification Head (2 classes)
     ↓
Softmax Output: [P(Healthy), P(LSD)]
     ↓
Argmax → Prediction Label
```

### Model Training Details
- **Dataset**: Cattle LSD images (binary classification)
- **Preprocessing**: 224×224 normalization, data augmentation
- **Optimizer**: Adam
- **Loss Function**: Categorical Crossentropy
- **Metrics**: Accuracy
- **Model Format**: Keras (.keras) with fallback to legacy format

### Model Files Location
```
backend/model/
├── lsd_final.keras          ← Production model (used by API)
├── lsd_model.keras          ← Alternative model
└── lsd_model_tf213.h5       ← TensorFlow 2.13 format
```

### Model Inference Process (Backend)

```python
# Load model at startup
model = load_model("backend/model/lsd_final.keras", compile=False)

# Preprocess image
image_bytes → PIL.Image → Resize(224,224) → Normalize

# Inference
predictions = model.predict(preprocessed_image)
confidence = max(predictions[0])
label = CLASS_NAMES[argmax(predictions[0])]

# Generate Recommendations
if label == "Lumpy Skin Disease":
    recommendations = [
        "Immediately isolate the animal from the herd.",
        "Contact a registered veterinarian as soon as possible.",
        "Administer prescribed anti-inflammatory medication.",
        "Apply insect/vector control measures in the barn.",
        "Report to local livestock disease authority."
    ]
```

---

## 📦 Installation & Setup

### Prerequisites
- **Python** 3.11+
- **Node.js** 16+ & npm
- **Git**
- **Docker** (optional, for containerization)

### 1. Clone Repository

```bash
git clone https://github.com/yourusername/LumpiScan.git
cd LumpiScan
```

### 2. Backend Setup

```bash
cd backend

# Create Python virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Verify model file exists
# Ensure backend/model/lsd_final.keras is present
ls model/

# Run Flask development server
python app.py
# Server starts at http://localhost:5000
```

### 3. Frontend Setup

```bash
cd ../frontend

# Install dependencies
npm install

# Start development server
npm run dev
# Frontend available at http://localhost:3000
```

### 4. Access Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **Health Check**: http://localhost:5000/

---

## 🚀 API Documentation

### Health Check

```http
GET /
```

**Response** (200 OK):
```json
{
  "status": "CattleCare API running",
  "model_loaded": true,
  "model_path": "/path/to/lsd_final.keras",
  "model_error": null
}
```

---

### Predict (Disease Detection)

```http
POST /predict
Content-Type: multipart/form-data

Body:
- image: <binary_image_file>
- user_id: <optional_user_id>
```

**Response** (200 OK):
```json
{
  "prediction": "Healthy" | "Lumpy Skin Disease",
  "confidence": 95.27,
  "is_infected": false,
  "recommendations": [
    "Animal appears healthy. Continue regular monitoring.",
    "Maintain vaccination schedule.",
    "Ensure clean water and proper nutrition."
  ],
  "case_id": "abc123def456",
  "saved_image": "filename.jpg"
}
```

---

### Register User (Cattle Owner)

```http
POST /register-user
Content-Type: application/json

{
  "phone": "9876543210",
  "location": "Pune, Maharashtra",
  "name": "Farmer Name (optional)"
}
```

**Response** (201 Created):
```json
{
  "message": "Registered successfully",
  "user_id": "uuid-string",
  "name": "Farmer Name",
  "location": "Pune, Maharashtra",
  "phone": "9876543210"
}
```

---

### Login User

```http
POST /login-user
Content-Type: application/json

{
  "phone": "9876543210"
}
```

**Response** (200 OK):
```json
{
  "message": "Login successful",
  "user_id": "uuid-string",
  "name": "Farmer Name",
  "location": "Pune, Maharashtra",
  "phone": "9876543210"
}
```

---

### Register Veterinarian

```http
POST /register-vet
Content-Type: application/json

{
  "name": "Dr. Raj Kumar",
  "phone": "9876543210",
  "specialization": "Livestock Medicine",
  "clinic_address": "Bangalore, Karnataka",
  "lat": 12.9716,
  "lon": 77.5946
}
```

**Response** (201 Created):
```json
{
  "message": "Veterinarian registered",
  "vet_id": "uuid-string"
}
```

---

### Search Veterinarians

```http
GET /search-doctors?location=Bangalore&lat=12.9716&lon=77.5946&radius_km=50
```

**Query Parameters**:
- `location` (string) – Location text search (clinic address, clinic name)
- `lat` (float) – User latitude (optional, for distance calculation)
- `lon` (float) – User longitude (optional, for distance calculation)
- `radius_km` (float) – Search radius in kilometers (default: 50)

**Response** (200 OK):
```json
{
  "vets": [
    {
      "id": "uuid-string",
      "name": "Dr. Raj Kumar",
      "phone": "9876543210",
      "specialization": "Livestock Medicine",
      "clinic_address": "Bangalore, Karnataka",
      "rating": 4.5,
      "reviews": 42,
      "distance_km": 5.2
    }
  ],
  "count": 1
}
```

---

### Get Prediction History

```http
GET /history/<user_id>
```

**Response** (200 OK):
```json
{
  "history": [
    {
      "id": "uuid-string",
      "user_id": "uuid-string",
      "filename": "image.jpg",
      "label": "Healthy",
      "confidence": 92.5,
      "is_infected": false,
      "timestamp": "2024-05-06T10:30:00.000Z"
    }
  ],
  "total": 5
}
```

---

## 💻 Usage Guide

### For Farmers

1. **Register/Login**
   - Navigate to "Login" page
   - Enter phone number
   - Receive registration confirmation

2. **Upload Cattle Image**
   - Click "Start Detection"
   - Drag & drop image or select from device
   - Wait for AI analysis (~3 seconds)

3. **View Results**
   - See disease prediction with confidence score
   - Read health recommendations
   - Option to contact veterinarian

4. **Find Veterinarian**
   - Go to "Veterinary" section
   - Search by location or click "Nearby"
   - View vet profiles, ratings, contact info
   - Call or book appointment

5. **Track History**
   - Visit "History" page
   - View all previous predictions
   - Filter by date or disease status

### For Veterinarians

1. **Register as Vet**
   - Click "Register as Veterinarian"
   - Enter clinic details and credentials
   - Provide GPS coordinates
   - Submit registration

2. **Receive Consultations**
   - Farmers contact you through the platform
   - Receive direct calls/booking requests
   - Provide remote consultation

---

## 🔧 Development

### Project Structure

```
LumpiScan/
├── backend/
│   ├── app.py                    ← Main Flask application
│   ├── convert_model.py          ← Model format conversion utility
│   ├── requirements.txt          ← Python dependencies
│   ├── Dockerfile               ← Docker configuration
│   ├── model/
│   │   ├── lsd_final.keras      ← Production model
│   │   ├── lsd_model.keras
│   │   └── lsd_model_tf213.h5
│   ├── uploads/                 ← Uploaded images storage
│   └── database.json            ← Persistent data store
│
├── frontend/
│   ├── src/
│   │   ├── App.jsx              ← Main React component
│   │   ├── main.jsx             ← React entry point
│   │   ├── index.css            ← Global styles
│   │   ├── animations.css       ← Custom animations
│   │   ├── components/
│   │   │   ├── Navbar.jsx
│   │   │   └── LanguageSwitcher.jsx
│   │   ├── context/
│   │   │   ├── AuthContext.jsx  ← User authentication
│   │   │   ├── LanguageContext.jsx
│   │   │   └── ThemeContext.jsx ← Dark mode
│   │   ├── hooks/
│   │   │   └── useScrollReveal.jsx
│   │   ├── i18n/
│   │   │   └── translations.js  ← Multi-language strings
│   │   └── pages/
│   │       ├── Home.jsx
│   │       ├── Detection.jsx
│   │       ├── Veterinary.jsx
│   │       ├── History.jsx
│   │       └── Auth.jsx
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   ├── vercel.json             ← Vercel deployment config
│   └── index.html
│
├── ml_model/
│   ├── train_model.py          ← Model training script
│   └── lsd_model_tf213.h5      ← Original model format
│
└── README.md                   ← This file
```

### Running Development Servers

**Terminal 1 - Backend**:
```bash
cd backend
source venv/bin/activate  # or venv\Scripts\activate on Windows
python app.py
```

**Terminal 2 - Frontend**:
```bash
cd frontend
npm run dev
```

### Building for Production

**Backend**:
```bash
cd backend
pip install -r requirements.txt
# Use Gunicorn
gunicorn --bind 0.0.0.0:10000 app:app
```

**Frontend**:
```bash
cd frontend
npm run build
# Outputs to frontend/dist/
```

### Debugging

**Backend Debugging**:
- Check Flask logs in terminal
- Verify model file exists at `backend/model/lsd_final.keras`
- Test endpoints using curl or Postman
- Check `database.json` for stored data

**Frontend Debugging**:
- Open browser DevTools (F12)
- Check Console tab for errors
- Network tab to inspect API calls
- Check Vite dev server logs

---

## 🐳 Docker Deployment

### Build Docker Image

```bash
cd backend
docker build -t lumpiscan-backend:latest .
```

### Run Container

```bash
docker run -p 10000:10000 \
  -v $(pwd)/uploads:/app/uploads \
  -v $(pwd)/database.json:/app/database.json \
  lumpiscan-backend:latest
```

### Docker Compose (Optional)

Create `docker-compose.yml`:

```yaml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "10000:10000"
    volumes:
      - ./backend/uploads:/app/uploads
      - ./backend/database.json:/app/database.json
    environment:
      - FLASK_ENV=production
  
  frontend:
    image: node:16-alpine
    working_dir: /app
    volumes:
      - ./frontend:/app
    ports:
      - "3000:3000"
    command: sh -c "npm install && npm run dev"
```

Run with:
```bash
docker-compose up
```

---

## 🌐 Deployment Platforms

### Vercel (Frontend)

1. Push frontend code to GitHub
2. Connect repo to Vercel
3. Set build command: `npm run build`
4. Set output directory: `dist`
5. Set environment variables (API URL)

### Render/Railway (Backend)

1. Push backend code to GitHub
2. Create new service on platform
3. Set build command: `pip install -r requirements.txt`
4. Set start command: `gunicorn --bind 0.0.0.0:10000 app:app`
5. Add volume for uploads and database persistence

### Environment Variables

Create `.env` file:

```
# Frontend (.env.local)
VITE_API_URL=https://your-backend-api.com

# Backend (.env)
FLASK_ENV=production
DEBUG=False
```

---

## 📊 Database Schema

### users Collection
```json
{
  "id": "uuid-string",
  "phone": "9876543210",
  "location": "Pune, Maharashtra",
  "name": "Farmer Name",
  "created_at": "2024-05-06T10:30:00.000Z"
}
```

### vets Collection
```json
{
  "id": "uuid-string",
  "name": "Dr. Raj Kumar",
  "phone": "9876543210",
  "specialization": "Livestock Medicine",
  "clinic_address": "Bangalore, Karnataka",
  "lat": 12.9716,
  "lon": 77.5946,
  "rating": 4.5,
  "reviews": 42,
  "created_at": "2024-05-06T10:30:00.000Z"
}
```

### predictions Collection
```json
{
  "id": "uuid-string",
  "user_id": "uuid-string",
  "filename": "image.jpg",
  "label": "Healthy",
  "confidence": 92.5,
  "is_infected": false,
  "timestamp": "2024-05-06T10:30:00.000Z"
}
```

---

## ⚙️ Configuration

### Image Processing
- **Input Size**: 224×224 pixels
- **Color Space**: RGB
- **Format Supported**: JPG, PNG
- **Max File Size**: Recommended <5MB

### Model Configuration
- **Model Path**: `backend/model/lsd_final.keras`
- **Classes**: ["Healthy", "Lumpy Skin Disease"]
- **Framework**: TensorFlow/Keras 2.21.0
- **Fallback**: Legacy Keras format support

### Geo-Location
- **Distance Calculation**: Haversine formula
- **Earth Radius**: 6,371 km
- **Default Search Radius**: 50 km

### Multi-Language Support
- **Available Languages**: English, Hindi
- **Translations File**: `frontend/src/i18n/translations.js`
- **Fallback Language**: English

---

## 🤝 Contributing

### Bug Reports & Feature Requests

1. Open an issue on GitHub
2. Provide detailed description
3. Include screenshots/logs if applicable

### Development Workflow

1. Fork repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Make changes and test
4. Commit with descriptive messages: `git commit -m 'Add amazing feature'`
5. Push to branch: `git push origin feature/amazing-feature`
6. Open Pull Request

### Code Standards

- **Python**: PEP 8 style guide
- **JavaScript**: ESLint configuration
- **Naming**: Descriptive, camelCase for JS, snake_case for Python
- **Comments**: Clear, concise documentation
- **Testing**: Ensure new features are tested

---

## 📝 License

This project is licensed under the MIT License - see LICENSE file for details.

---

## 👥 Team & Contributors

- **Project Lead**: [Your Name]
- **ML Engineer**: [Team Member]
- **Full Stack Developer**: [Team Member]
- **UI/UX Designer**: [Team Member]

---

## 📞 Support & Contact

- **Email**: support@lumpiscan.com
- **Website**: https://lumpiscan.com
- **Issues**: GitHub Issues
- **Documentation**: https://docs.lumpiscan.com

---

## 🙏 Acknowledgments

- **TensorFlow & Keras** – ML frameworks
- **React & Vite** – Frontend technologies
- **Flask** – Backend framework
- **Tailwind CSS** – UI styling
- **Farming community** – For testing and feedback

---

## 📚 Additional Resources

### Learning Materials
- [MobileNetV2 Paper](https://arxiv.org/abs/1801.04381)
- [Lumpy Skin Disease - FAO](http://www.fao.org)
- [TensorFlow Documentation](https://tensorflow.org/guide)
- [React Documentation](https://react.dev)

### Related Projects
- [Animal Disease Detection AI](https://github.com/example/animal-disease-ai)
- [Veterinary Management System](https://github.com/example/vet-management)

---

**Last Updated**: May 6, 2024  
**Version**: 1.0.0  
**Status**: Production Ready ✅

---

## 🚀 Quick Start Cheat Sheet

```bash
# Backend
cd backend && source venv/bin/activate && python app.py

# Frontend
cd frontend && npm run dev

# Production Build
cd frontend && npm run build

# Docker
docker build -t lumpiscan-backend . && docker run -p 10000:10000 lumpiscan-backend

# Test API
curl http://localhost:5000/
```

---

**Made with ❤️ for farmers & veterinarians worldwide.**
