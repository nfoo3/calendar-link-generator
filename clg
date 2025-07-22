from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from fastapi.middleware.cors import CORSMiddleware
from datetime import datetime, timedelta
import re

app = FastAPI()

# Allow CORS for local development and testing
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

class EventText(BaseModel):
    event_text: str

@app.post("/generate-calendar-link")
def generate_calendar_link(payload: EventText):
    # Split lines and extract event name and date
    lines = [line.strip() for line in payload.event_text.strip().split('\n') if line.strip()]
    if len(lines) < 2:
        raise HTTPException(status_code=400, detail="Please provide event name and date on separate lines.")
    event_name = lines[0]
    date_str = lines[1]

    # Try to parse the date (assume EST)
    try:
        # Accepts formats like: April 30, 2024 2:00 PM EST
        match = re.match(r"([A-Za-z]+ \d{1,2}, \d{4} \d{1,2}:\d{2} ?[APMapm]{2}) ?(EST)?", date_str)
        if not match:
            raise ValueError("Date format not recognized.")
        dt = datetime.strptime(match.group(1), "%B %d, %Y %I:%M %p")
        # EST is UTC-5, but if in daylight saving, use EDT (UTC-4). For simplicity, use UTC-5.
        dt_utc = dt + timedelta(hours=5)  # Convert EST to UTC
        # Default to 1 hour event
        dt_utc_end = dt_utc + timedelta(hours=1)
        start_str = dt_utc.strftime("%Y%m%dT%H%M%SZ")
        end_str = dt_utc_end.strftime("%Y%m%dT%H%M%SZ")
    except Exception as e:
        raise HTTPException(status_code=400, detail=f"Could not parse date: {e}")

    # Build Google Calendar link
    base_url = "https://calendar.google.com/calendar/render?action=TEMPLATE"
    params = f"&text={event_name.replace(' ', '+')}&dates={start_str}/{end_str}"
    calendar_link = base_url + params
    return {"calendar_link": calendar_link}

@app.get("/")
def read_root():
    return {"message": "Calendar Link Generator API", "docs": "/docs"}
