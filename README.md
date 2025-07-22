# Calendar Link Generator

A FastAPI application that generates Google Calendar links from event text input.

## Features

- ✅ **FastAPI Framework**: Modern, fast web framework for Python
- ✅ **Event Parsing**: Extracts event name and date from text input
- ✅ **Date Format Support**: Parses "Month DD, YYYY H:MM AM/PM EST" format
- ✅ **EST to UTC Conversion**: Automatically converts Eastern time to UTC
- ✅ **Google Calendar Integration**: Generates valid Google Calendar URLs
- ✅ **Default Duration**: Sets 1-hour default event duration
- ✅ **CORS Support**: Enabled for local development and testing
- ✅ **Error Handling**: Comprehensive error messages for invalid input
- ✅ **URL Encoding**: Proper encoding of special characters in event names

## Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Setup

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd calendar-link-generator
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application**:
   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000
   ```

   The API will be available at `http://localhost:8000`

## Usage

### API Endpoints

#### `POST /generate-calendar-link`

Generate a Google Calendar link from event text.

**Request Body**:
```json
{
  "event_text": "Event Name\nApril 30, 2024 2:00 PM EST"
}
```

**Response**:
```json
{
  "calendar_link": "https://calendar.google.com/calendar/render?action=TEMPLATE&text=Event+Name&dates=20240430T190000Z/20240430T200000Z"
}
```

#### `GET /`

Get API information and status.

**Response**:
```json
{
  "message": "Calendar Link Generator API",
  "docs": "/docs",
  "version": "1.0.0"
}
```

### Input Format

The `event_text` should contain:
1. **Line 1**: Event name
2. **Line 2**: Date in format "Month DD, YYYY H:MM AM/PM EST"

**Example**:
```
Team Meeting
December 15, 2024 3:30 PM EST
```

### Example Usage

#### Using curl

```bash
curl -X POST "http://localhost:8000/generate-calendar-link" \
  -H "Content-Type: application/json" \
  -d '{"event_text": "Team Meeting\nDecember 15, 2024 3:30 PM EST"}'
```

#### Using Python requests

```python
import requests
import json

url = "http://localhost:8000/generate-calendar-link"
data = {
    "event_text": "Team Meeting\nDecember 15, 2024 3:30 PM EST"
}

response = requests.post(url, json=data)
result = response.json()
print(result["calendar_link"])
```

### Interactive API Documentation

Visit `http://localhost:8000/docs` for interactive Swagger UI documentation.

## Error Handling

The API returns appropriate error codes and messages:

- **400 Bad Request**: Invalid input format or unparseable dates
  - Missing event name or date
  - Incorrect date format
  - Invalid date values

**Example Error Response**:
```json
{
  "detail": "Could not parse date: Date format not recognized. Expected format: 'Month DD, YYYY H:MM AM/PM EST'"
}
```

## Development

### Project Structure

```
calendar-link-generator/
├── main.py              # Main FastAPI application
├── requirements.txt     # Python dependencies
├── README.md           # This file
└── clg                 # Original implementation (legacy)
```

### Dependencies

- **FastAPI**: Web framework
- **Pydantic**: Data validation
- **Uvicorn**: ASGI server
- **Standard Libraries**: datetime, re, urllib.parse

### Running in Development Mode

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

## Time Zone Handling

- The application expects EST (Eastern Standard Time) input
- Converts EST to UTC by adding 5 hours
- Generated calendar links use UTC format (Z suffix)
- Note: Does not account for daylight saving time transitions

## Supported Date Formats

Currently supports:
- `"April 30, 2024 2:00 PM EST"`
- `"December 15, 2024 11:30 AM EST"`
- `"January 1, 2025 8:45 PM EST"`

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is open source and available under the MIT License.