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
2. **Smart Image Validation**: A dedicated AI model first verifies the uploaded image actually contains a cow before running disease analysis, preventing false results on irrelevant images
3. **Accessibility**: Works on mobile/web browsers without requiring physical veterinary visits
4. **Professional Network Integration**: Direct connection to registered veterinarians for consultation
5. **Digital Health Records**: Complete prediction history for tracking animal health over time
6. **Multi-language Support**: Available in English & Hindi for farmer accessibility

### Impact
- ✅ **Early Detection**: Identify LSD within minutes instead of days
- ✅ **Disease Control**: Isolate infected animals before herd-wide spread
- ✅ **Cost Reduction**: Prevent economic losses through preventive measures
- ✅ **Remote Care**: Connect rural farmers with expert veterinarians instantly
- ✅ **Data-Driven**: Digital records enable better herd management
- ✅ **Input Validation**: Reject non-cattle images instantly, preventing misleading diagnoses

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
│  • Cow Validation (Stage 1 AI)                         │
│  • LSD Classification (Stage 2 AI)                     │
│  • User & Veterinarian Management                       │
│  • Prediction History Storage                           │
│  • Veterinarian Search & Geo-location                   │
└────────────────┬────────────────────────────────────────┘
                 │
    ┌────────────┼──────────────────┐
    │            │                  │
┌───▼────────────▼──┐  ┌────▼────┐  ┌────▼────┐
│   ML Models       │  │Database │  │Uploads  │
│ • cow_or_not.keras│  │(JSON)   │  │(Images) │
│ • lsd_final.keras │  └─────────┘  └─────────┘
└───────────────────┘
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
- `GET /` – Health check (reports both model statuses)
- `POST /predict` – Image upload → cow validation → disease prediction
- `POST /register-user` – Register cattle owner
- `POST /login-user` – Login for cattle owners
- `POST /register-vet` – Register veterinarian
- `GET /search-doctors` – Find veterinarians by location
- `GET /history/<user_id>` – Fetch prediction history

#### ML Models (CNN - MobileNetV2)

LumpiScan uses a **two-stage AI pipeline**:

**Stage 1 — Cow Validator (`cow_or_not_final.keras`)**
- Checks whether the uploaded image contains a cow
- Rejects non-cattle images instantly with a clear error message
- Accuracy: ~99%
- Prevents the LSD model from running on irrelevant images (people, objects, other animals)

**Stage 2 — LSD Classifier (`lsd_final.keras`)**
- Only runs if Stage 1 confirms a cow is present
- Classifies the cow as Healthy or Lumpy Skin Disease
- Accuracy: ~95.15%
- Inference Time: <3 seconds per image

---

## 🧠 ML Model Architecture

### Two-Stage AI Pipeline

```
User uploads image
        │
        ▼
┌───────────────────────────┐
│  Stage 1: Cow Validator   │
│  cow_or_not_final.keras   │
│  MobileNetV2 + Sigmoid    │
│  Accuracy: ~99%           │
└───────────┬───────────────┘
            │
     ┌──────┴──────┐
     │             │
  Not a cow      Is a cow
     │             │
     ▼             ▼
 422 Error    ┌───────────────────────────┐
 "Invalid     │  Stage 2: LSD Classifier  │
  image"      │  lsd_final.keras          │
              │  MobileNetV2 + Sigmoid    │
              │  Accuracy: ~95.15%        │
              └───────────┬───────────────┘
                          │
               ┌──────────┴──────────┐
               │                     │
            Healthy            Lumpy Skin
               │                  Disease
               ▼                     ▼
        Monitoring             Isolation &
        recommendations        Vet referral
```

### Why MobileNetV2?
- Lightweight (~3–4MB) – suitable for mobile deployment
- Fast inference (<3 seconds) – real-time predictions
- Transfer learning friendly – pretrained on ImageNet weights
- High accuracy – state-of-the-art for image classification

### Model Training Details

| Property | Cow Validator | LSD Classifier |
|---|---|---|
| File | `cow_or_not_final.keras` | `lsd_final.keras` |
| Task | Binary (cow / not cow) | Binary (healthy / LSD) |
| Output | Sigmoid (1 neuron) | Sigmoid (1 neuron) |
| Accuracy | ~99% | ~95.15% |
| Input Size | 224×224 RGB | 224×224 RGB |
| Base Model | MobileNetV2 | MobileNetV2 |
| Optimizer | Adam | Adam |
| Loss | Binary Crossentropy | Binary Crossentropy |

### Model Files Location
```
backend/model/
├── cow_or_not_final.keras   ← Stage 1: Cow/not-cow validator (99% accuracy)
└── lsd_final.keras          ← Stage 2: LSD classifier (95% accuracy)
```

### Model Inference Process (Backend)

```python
# Stage 1 — Validate image contains a cow
cow_score = cow_model.predict(preprocessed_image)[0][0]
if cow_score >= 0.5:          # high score = not a cow (model is inverted)
    return 422 "Invalid image — not a cow"

# Stage 2 — Classify disease (only runs if cow confirmed)
lsd_score = model.predict(preprocessed_image)[0][0]
if lsd_score >= 0.5:
    label = "Lumpy Skin Disease"
    confidence = lsd_score
else:
    label = "Healthy"
    confidence = 1.0 - lsd_score
```

---

## 🎨 Features

### 1. **Smart Image Validation**
- Dedicated cow-detection AI model runs before any disease analysis
- Non-cattle images (people, objects, other animals) are rejected immediately
- Returns a clear `422 Invalid image` response with a user-friendly message
- Only valid cattle photos proceed to disease classification
- Prevents misleading or nonsensical diagnoses

### 2. **AI Disease Detection**
- Upload cattle image (JPG/PNG)
- Real-time two-stage inference using MobileNetV2 CNN
- Confidence score display
- Health recommendations based on prediction
- Case ID for tracking

### 3. **Veterinary Network**
- Search vets by name, clinic, or location
- GPS-based distance calculation (Haversine formula)
- Vet ratings and reviews
- Direct phone contact
- Appointment booking interface
- Veterinarian registration portal

### 4. **Digital Health Records**
- Complete prediction history per user
- Timestamp tracking
- Confidence metrics
- Disease status tracking
- Re-examine previous cases

### 5. **User Management**
- Phone-based registration & authentication
- Cattle owner profiles
- Veterinarian profiles
- Location-based services

### 6. **User Experience**
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
- **TensorFlow** 2.19.0 – ML framework
- **Keras** 3.10.0 – Neural network API
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

# Verify both model files exist
ls model/
# Expected output:
#   cow_or_not_final.keras
#   lsd_final.keras

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
  "model_error": null,
  "cow_model_loaded": true,
  "cow_model_error": null,
  "model_path": "/path/to/lsd_final.keras",
  "cow_model_path": "/path/to/cow_or_not_final.keras"
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

**Stage 1 fails — not a cow** (422 Unprocessable Entity):
```json
{
  "error": "Invalid image",
  "message": "The uploaded image does not appear to contain a cow. Please upload a clear photo of the animal.",
  "is_valid_cow": false
}
```

**Stage 2 success — cow detected and classified** (200 OK):
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
  "total": 1
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
   - **Important**: Upload a clear photo of your cow — other images will be rejected
   - Wait for AI analysis (~3 seconds)

3. **View Results**
   - If image is not a cow: receive an "Invalid image" message prompting a correct upload
   - If image is a cow: see disease prediction with confidence score and health recommendations
   - Option to contact veterinarian if LSD is detected

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
│   ├── app.py                        ← Main Flask application
│   ├── fix_models.py                 ← Model compatibility fix utility
│   ├── requirements.txt              ← Python dependencies
│   ├── Dockerfile                    ← Docker configuration
│   ├── model/
│   │   ├── cow_or_not_final.keras   ← Stage 1: Cow validator (99% accuracy)
│   │   └── lsd_final.keras          ← Stage 2: LSD classifier (95% accuracy)
│   ├── uploads/                      ← Uploaded images storage
│   └── database.json                 ← Persistent data store
│
├── frontend/
│   ├── src/
│   │   ├── App.jsx                  ← Main React component
│   │   ├── main.jsx                 ← React entry point
│   │   ├── index.css                ← Global styles
│   │   ├── animations.css           ← Custom animations
│   │   ├── components/
│   │   │   ├── Navbar.jsx
│   │   │   └── LanguageSwitcher.jsx
│   │   ├── context/
│   │   │   ├── AuthContext.jsx      ← User authentication
│   │   │   ├── LanguageContext.jsx
│   │   │   └── ThemeContext.jsx     ← Dark mode
│   │   ├── hooks/
│   │   │   └── useScrollReveal.jsx
│   │   ├── i18n/
│   │   │   └── translations.js      ← Multi-language strings
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
│   ├── vercel.json                  ← Vercel deployment config
│   └── index.html
│
├── ml_model/
│   ├── train_model.py               ← LSD model training script
│   └── train_cow_validator.py       ← Cow validator training script
│
└── README.md                        ← This file
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
- Check Flask logs in terminal — startup logs report both model load statuses
- Verify both model files exist at `backend/model/`
- Hit `GET /` to confirm `cow_model_loaded: true` and `model_loaded: true`
- Test with Postman: send a non-cow image and verify 422 response
- Test with a cow image and verify 200 response with prediction
- Check `database.json` for stored predictions

**Frontend Debugging**:
- Open browser DevTools (F12)
- Check Console tab for errors
- Network tab to inspect API calls — look for 422 vs 200 on `/predict`
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
6. Ensure both `.keras` model files are included in the repository or mounted as volumes

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
- **Formats Supported**: JPG, PNG
- **Max File Size**: Recommended <5MB
- **Preprocessing**: MobileNetV2 `preprocess_input` normalization

### Model Configuration

| Setting | Value |
|---|---|
| Cow Validator Path | `backend/model/cow_or_not_final.keras` |
| LSD Classifier Path | `backend/model/lsd_final.keras` |
| Classes (LSD model) | `["Healthy", "Lumpy Skin Disease"]` |
| Decision Threshold | `0.5` (sigmoid output) |
| Image Size | `224 × 224` |

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

**Last Updated**: September 2026
**Version**: 2.0.0
**Status**: Production Ready ✅

---

## 🚀 Quick Start Cheat Sheet

```bash
# Backend
cd backend && source venv/bin/activate && python app.py

# Frontend
cd frontend && npm run dev

# Verify both models loaded
curl http://localhost:5000/
# Check: "cow_model_loaded": true AND "model_loaded": true

# Test cow validation (should return 422)
curl -X POST http://localhost:5000/predict -F "image=@non_cow_image.jpg"

# Test full pipeline (should return prediction)
curl -X POST http://localhost:5000/predict -F "image=@cow_image.jpg"

# Production Build
cd frontend && npm run build

# Docker
docker build -t lumpiscan-backend . && docker run -p 10000:10000 lumpiscan-backend
```

---

**Made with ❤️ for farmers & veterinarians worldwide.**
