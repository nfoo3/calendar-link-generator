# Calendar Link Generator

A FastAPI application that generates Google Calendar links from event text input. Simply provide an event name and date, and get back a Google Calendar link that can be shared with others.

## Features

- **Simple API**: Send event text and get a Google Calendar link
- **Date Parsing**: Supports various date formats (e.g., "April 30, 2024 2:00 PM EST")
- **CORS Support**: Configured for web applications
- **Health Checks**: Built-in health monitoring
- **Containerized**: Docker and Docker Compose ready

## API Usage

### Generate Calendar Link

**Endpoint:** `POST /generate-calendar-link`

**Request Body:**
```json
{
  "event_text": "Meeting Title\nApril 30, 2024 2:00 PM EST"
}
```

**Response:**
```json
{
  "calendar_link": "https://calendar.google.com/calendar/render?action=TEMPLATE&text=Meeting+Title&dates=20240430T190000Z/20240430T200000Z"
}
```

### Health Check

**Endpoint:** `GET /`

**Response:**
```json
{
  "message": "Calendar Link Generator API",
  "docs": "/docs"
}
```

### API Documentation

Visit `/docs` for interactive Swagger documentation when the server is running.

## Local Development

### Prerequisites

- Python 3.11+
- pip

### Setup

1. Clone the repository
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the application:
   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000
   ```

4. Access the API at `http://localhost:8000`

## Docker Deployment

### Prerequisites

- Docker
- Docker Compose (optional)

### Option 1: Docker Build and Run

1. Build the Docker image:
   ```bash
   docker build -t calendar-link-generator .
   ```

2. Run the container:
   ```bash
   docker run -p 8000:8000 calendar-link-generator
   ```

3. Access the API at `http://localhost:8000`

### Option 2: Docker Compose (Recommended)

1. Start the application:
   ```bash
   docker compose up -d
   ```

2. Stop the application:
   ```bash
   docker compose down
   ```

3. Access the API at `http://localhost:8000`

### Docker Features

- **Security**: Runs as non-root user
- **Health Checks**: Automatic health monitoring
- **Optimized Build**: Uses .dockerignore for faster builds
- **Python 3.11**: Latest stable Python version

## Cloud Deployment

### Docker Hub

1. Tag your image:
   ```bash
   docker tag calendar-link-generator your-username/calendar-link-generator
   ```

2. Push to Docker Hub:
   ```bash
   docker push your-username/calendar-link-generator
   ```

### Platform Deployment

This application can be deployed on various cloud platforms:

- **Heroku**: Use the Dockerfile for container deployment
- **AWS ECS/Fargate**: Deploy the Docker image
- **Google Cloud Run**: Deploy the containerized application
- **Azure Container Instances**: Use the Docker image
- **Railway/Render**: Direct Docker deployment

### Environment Variables

The application currently uses default settings but can be extended with:

- `PORT`: Server port (default: 8000)
- `HOST`: Server host (default: 0.0.0.0)

## API Examples

### Using curl

```bash
# Health check
curl http://localhost:8000/

# Generate calendar link
curl -X POST http://localhost:8000/generate-calendar-link \
  -H "Content-Type: application/json" \
  -d '{"event_text": "Team Meeting\nApril 30, 2024 2:00 PM EST"}'
```

### Using Python requests

```python
import requests

# Generate calendar link
response = requests.post(
    "http://localhost:8000/generate-calendar-link",
    json={"event_text": "Team Meeting\nApril 30, 2024 2:00 PM EST"}
)

print(response.json())
# Output: {"calendar_link": "https://calendar.google.com/calendar/render?action=TEMPLATE&text=Team+Meeting&dates=20240430T190000Z/20240430T200000Z"}
```

### Using JavaScript fetch

```javascript
fetch('http://localhost:8000/generate-calendar-link', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    event_text: 'Team Meeting\nApril 30, 2024 2:00 PM EST'
  })
})
.then(response => response.json())
.then(data => console.log(data.calendar_link));
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with Docker
5. Submit a pull request

## License

This project is open source and available under the MIT License.