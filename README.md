# Full Stack Developer Capstone Project

This repository contains the capstone project for the IBM Full Stack Software Developer
Professional Certificate. It is a dealership review application built with a Django
backend, a React frontend, a Node.js/Express/MongoDB microservice for dealer and review
data, and a Flask-based sentiment analysis microservice.

## Project structure

- `server/` — Django project (`djangoproj`) and app (`djangoapp`) serving the backend
  API, admin site, and the built React frontend.
- `server/frontend/` — React single-page application (login, registration, dealer
  listing, dealer details/reviews, and review submission pages).
- `server/database/` — Node.js/Express service backed by MongoDB, exposing endpoints
  to fetch dealers and reviews and to insert new reviews.
- `server/djangoapp/microservices/` — Flask microservice using NLTK's VADER sentiment
  analyzer, deployable as a standalone container (e.g. on IBM Cloud Code Engine).

## Features

- User registration, login, and logout.
- Browse car dealerships, filter by state.
- View a dealership's details and its customer reviews, each annotated with a
  sentiment (positive/neutral/negative) computed by the sentiment analysis
  microservice.
- Authenticated users can submit a review for a dealership, including the car
  make/model/year purchased.
- Django admin site for managing `CarMake` and `CarModel` records.

## Running locally

1. Set up the Django backend:
   ```bash
   cd server
   python3 -m venv djangoenv
   source djangoenv/bin/activate
   pip install -r requirements.txt
   python3 manage.py makemigrations
   python3 manage.py migrate --run-syncdb
   ```
2. Build the React frontend:
   ```bash
   cd server/frontend
   npm install
   npm run build
   ```
3. Start the Node/MongoDB backend:
   ```bash
   cd server/database
   docker-compose up -d
   ```
4. Configure `server/djangoapp/.env` with `backend_url` (the Node service above) and
   `sentiment_analyzer_url` (the Flask sentiment microservice).
5. Run the Django development server:
   ```bash
   cd server
   python3 manage.py runserver 8000
   ```

## Deployment

The Django app can be containerized using `server/Dockerfile` and deployed to
Kubernetes via a Deployment manifest. The sentiment analysis microservice has its
own `Dockerfile` under `server/djangoapp/microservices/` for deployment as an
independent service (e.g. IBM Cloud Code Engine).

## CI

A GitHub Actions workflow (`.github/workflows/main.yml`) lints Python code with
`flake8` and JavaScript code (under `server/database/`) with `jshint` on every push
and pull request to `main`.
