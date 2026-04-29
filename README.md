# White Blood Cell Classification System

This project is a web application for white blood cell image classification.

The user uploads a microscope image in the frontend. The backend loads a trained Keras model and predicts the cell type. The system returns the predicted class, the confidence score, and the probability for all classes.

## Project Goal

The goal of this project is to help classify white blood cell images into 5 classes:

- Neutrophil
- Lymphocyte
- Monocyte
- Eosinophil
- Basophil

This project uses:

- React + Vite for the frontend
- FastAPI + TensorFlow for the backend
- A trained `.keras` model saved from Google Colab

## Main Features

- Upload a microscope image from the browser
- Send the image to a Python backend
- Load a trained Keras model for prediction
- Return the predicted class
- Return confidence and full probability scores
- Show the result in a simple frontend interface

## Project Structure

```text
WBC2/
├── backend/
│   ├── main.py
│   └── requirements.txt
├── frontend/
│   ├── package.json
│   ├── src/
│   │   ├── components/
│   │   ├── lib/
│   │   ├── pages/
│   │   └── main.tsx
├── models/
│   └── wbc_classification_model.keras
├── saved_code/
├── white-blood-cells-dataset/
├── venv/
└── README.md
```

## How The System Works

1. The user opens the frontend in the browser.
2. The user uploads an image.
3. The frontend sends the image to the backend with a `POST /predict` request.
4. The backend reads the image.
5. The backend resizes the image to `128 x 128`.
6. The backend changes the image to RGB.
7. The backend normalizes pixel values by dividing by `255.0`.
8. The model predicts the result.
9. The backend returns JSON data to the frontend.
10. The frontend shows the prediction and confidence.

## Model Information

The model file is stored here:

`models/wbc_classification_model.keras`

The backend uses this class order:

```text
Neutrophil
Lymphocyte
Monocyte
Eosinophil
Basophil
```

Input image settings:

- Image size: `128 x 128`
- Color mode: `RGB`
- Normalization: `image / 255.0`

## Requirements

Before you run the project, make sure you have:

- Python 3.11
- Node.js and npm
- A working internet connection for the first install

## Backend Setup

The backend is in one file:

`backend/main.py`

### 1. Create or activate the virtual environment

If you already have a good virtual environment, activate it:

```powershell
cd C:\Users\user\Desktop\WBC2
.\venv\Scripts\Activate.ps1
```

If the virtual environment is broken, delete it and create a new one:

```powershell
cd C:\Users\user\Desktop\WBC2
Remove-Item -Recurse -Force .\venv
python -m venv venv
.\venv\Scripts\Activate.ps1
```

### 2. Install backend packages

```powershell
cd C:\Users\user\Desktop\WBC2
.\venv\Scripts\Activate.ps1
pip install -r .\backend\requirements.txt
```

### 3. Run the backend

```powershell
cd C:\Users\user\Desktop\WBC2
.\venv\Scripts\Activate.ps1
cd .\backend
python -m uvicorn main:app --reload --port 8000
```

### 4. Check the backend

Open these links in your browser:

- `http://127.0.0.1:8000`
- `http://127.0.0.1:8000/docs`
- `http://127.0.0.1:8000/health`

If the backend works, you should see a message or a JSON response.

## Frontend Setup

### 1. Install frontend packages

```powershell
cd C:\Users\user\Desktop\WBC2\frontend
npm install
```

### 2. Run the frontend

```powershell
cd C:\Users\user\Desktop\WBC2\frontend
npm run dev
```

Vite may use:

- `http://localhost:8080`
- or `http://localhost:8081`

Use the exact local URL shown in the terminal.

## Full Run Steps

Use two terminals.

### Terminal 1: Backend

```powershell
cd C:\Users\user\Desktop\WBC2
.\venv\Scripts\Activate.ps1
cd .\backend
python -m uvicorn main:app --reload --port 8000
```

### Terminal 2: Frontend

```powershell
cd C:\Users\user\Desktop\WBC2\frontend
npm run dev
```

### Open in browser

Open the frontend URL from the terminal, for example:

- `http://localhost:8081`

Then:

1. Upload an image
2. Click `Run Prediction`
3. Wait for the result

## Backend API

### `GET /`

Simple message to check if the backend is running.

### `GET /health`

Returns backend health information.

Example response:

```json
{
  "status": "ok",
  "model_loaded": true
}
```

### `POST /predict`

Upload one image file and get the prediction result.

Form field:

- `file`

Example response:

```json
{
  "prediction": "Neutrophil",
  "confidence": 0.97,
  "probabilities": {
    "Neutrophil": 0.97,
    "Lymphocyte": 0.01,
    "Monocyte": 0.01,
    "Eosinophil": 0.00,
    "Basophil": 0.01
  }
}
```

## Backend Notes

The backend:

- loads the model at startup
- accepts image files only
- resizes the image to `128 x 128`
- converts the image to RGB
- normalizes the image
- runs model inference
- returns JSON output

The backend also allows frontend requests from local Vite ports such as:

- `localhost:8080`
- `localhost:8081`
- `localhost:5173`

## Frontend Notes

The frontend:

- lets the user upload an image
- sends the image to `http://127.0.0.1:8000/predict`
- shows errors if prediction fails
- shows prediction and probability results after success

Important note:

The helper file `frontend/src/lib/mockPrediction.ts` still supports mock mode, but the home page is already connected to the real backend.

## Important Files

### Backend

- `backend/main.py`
  The full backend code in one file.

- `backend/requirements.txt`
  Python packages for the backend.

### Frontend

- `frontend/src/pages/Index.tsx`
  Main page for upload and prediction.

- `frontend/src/lib/mockPrediction.ts`
  Sends the request to the backend and also contains old mock logic.

- `frontend/package.json`
  Frontend scripts and package list.

### Model

- `models/wbc_classification_model.keras`
  Trained model file used by the backend.

## How To Replace The Model

If you train a new model in Colab:

1. Save the model
2. Download the model file
3. Replace the old file in `models/`
4. Restart the backend

Example from Colab:

```python
model.save("wbc_classification_model.keras")
```

Then place the downloaded file here:

```text
WBC2/models/wbc_classification_model.keras
```

## Troubleshooting

### 1. `python` is not recognized

Python is not installed, or it is not in PATH.

Fix:

- Install Python 3.11
- Restart PowerShell
- Try again

### 2. Virtual environment is broken

This can happen if the project folder name changed.

Fix:

```powershell
cd C:\Users\user\Desktop\WBC2
Remove-Item -Recurse -Force .\venv
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r .\backend\requirements.txt
```

### 3. Frontend shows `NetworkError when attempting to fetch resource`

Possible reasons:

- backend is not running
- backend crashed during startup
- wrong backend URL
- CORS problem

Fix:

1. Make sure the backend terminal is still running
2. Open `http://127.0.0.1:8000`
3. Restart both backend and frontend

### 4. Model file not found

Make sure this file exists:

`models/wbc_classification_model.keras`

### 5. Port already in use

If frontend port `8080` is busy, Vite may move to `8081`.

This is normal. Open the URL shown in the terminal.

### 6. Backend starts but model does not load

This can happen if the model was saved with a different Keras version.

Fix:

- use the current backend requirements
- reinstall the backend packages
- if needed, save the model again from Colab and download it again

## Development Commands

### Frontend

```powershell
npm run dev
npm run build
npm run test
npm run lint
```

### Backend

```powershell
python -m uvicorn main:app --reload --port 8000
```

## Current Limitations

- The backend has no test suite yet
- The frontend has only simple test setup
- The model depends on local file loading
- The app runs for local development only

## Future Improvements

- add backend tests
- add better frontend error messages
- allow drag and drop for more image types
- add Docker support
- deploy frontend and backend online
- save prediction history

## Author Notes

This project combines machine learning and web development in one system. The model was trained in Google Colab, then moved to a local FastAPI backend, and connected to a React frontend.

The project is useful for learning:

- computer vision basics
- TensorFlow model serving
- FastAPI backend development
- React frontend integration

## License

This project is for educational use unless you add your own license.
