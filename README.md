# ✈️ AI Travel Planner

An intelligent travel planning application powered by AI agents and real-time data access through Model Context Protocol (MCP) servers.

## Overview

AI Travel Planner leverages advanced AI models to create highly detailed, personalized travel itineraries. By integrating with MCP servers and web services, it provides real-time access to current data including Airbnb listings, Google Maps location services, and travel information.

## Features

- **🤖 AI-Powered Planning**: Creates comprehensive travel itineraries using advanced language models (OpenAI or Ollama)
- **🏨 Real-Time Accommodation Data**: Access to Airbnb listings with current availability and pricing via MCP
- **🗺️ Location Services**: Google Maps integration for distance calculations, directions, and local navigation
- **🔍 Web Search Integration**: Current information, reviews, and travel updates
- **📅 Calendar Export**: Download itineraries as ICS calendar files
- **💰 Budget-Conscious**: Detailed cost breakdowns and accommodation recommendations within budget
- **🎯 Customizable Preferences**: Support for various travel styles (adventure, relaxation, cultural, etc.)
- **📊 Detailed Itineraries**: Day-by-day schedules with specific timings, distances, and recommendations

## Tech Stack

- **Frontend**: [Streamlit](https://streamlit.io) - Interactive web interface
- **AI Framework**: [Agno](https://github.com/phidatahq/agno) - Agentic AI framework
- **Models**: 
  - OpenAI GPT (via OpenRouter)
  - Ollama (local models like Qwen2.5:3b)
- **MCP Servers**:
  - Airbnb MCP (@openbnb/mcp-server-airbnb)
  - Travel Planner MCP (@gongrzhe/server-travelplanner-mcp)
  - Google Maps API
- **Calendar**: iCalendar format support
- **Dependencies**: See [requirements.txt](requirements.txt)

## Requirements

### System Requirements
- Python 3.12 or higher
- Node.js/npm (for MCP servers)
- Internet connection for API calls

### API Keys Required
1. **Google Maps API Key** - For location services and distance calculations
   - Get it from: https://console.cloud.google.com/apis/credentials
   - Required APIs: Maps JavaScript API, Directions API, Geocoding API
   
2. **OpenAI API Key** (optional, if not using local Ollama)
   - Get it from: https://platform.openai.com/api-keys
   - Or use OpenRouter: https://openrouter.ai

## Installation

1. **Clone the repository** (if applicable):
```bash
cd ~/projects/ai_travel_planner
```

2. **Create a virtual environment**:
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. **Install dependencies**:
```bash
pip install -r requirements.txt
```

Or using UV:
```bash
uv pip install -r requirements.txt
```

4. **Verify installation**:
```bash
python -c "from agno.tools.googlesearch import GoogleSearchTools; print('✅ Tools loaded')"
```

## Setup & Configuration

### 1. Environment Variables
You can set API keys as environment variables:
```bash
export OPENAI_API_KEY="your-openai-key"
export GOOGLE_MAPS_API_KEY="your-google-maps-key"
```

### 2. Model Selection
The app uses **Ollama (Qwen2.5:3b)** by default for local inference. To use OpenAI instead:
- Edit [app.py](app.py) line ~95
- Uncomment the OpenAIChat line and comment out the Ollama line

### 3. MCP Server Configuration
MCP servers are automatically spawned in the code:
- Airbnb MCP: `npx -y @openbnb/mcp-server-airbnb --ignore-robots-txt`
- Travel Planner MCP: `npx @gongrzhe/server-travelplanner-mcp`

Ensure `npm` is available and internet-connected.

## Usage

### Running the Application

```bash
streamlit run app.py
```

The app will open in your default browser at `http://localhost:8501`

### Using the Interface

1. **Enter API Keys** (in sidebar):
   - Google Maps API Key (required)
   - OpenAI API Key (required for OpenAI models, optional for Ollama)

2. **Specify Trip Details**:
   - Destination (e.g., "Paris", "Tokyo")
   - Number of days (1-30)
   - Budget in USD
   - Start date

3. **Set Travel Preferences**:
   - Describe preferences in text (food, adventure, cultural sites, etc.)
   - Or select quick preference tags (Adventure, Beach, Luxury, etc.)

4. **Generate Itinerary**:
   - Click "Generate Itinerary" button
   - Wait for AI to plan your trip (may take 1-2 minutes)
   - Review the detailed itinerary

5. **Download Calendar**:
   - Click "Download as Calendar" to save as .ics file
   - Import into your calendar app (Outlook, Google Calendar, etc.)

## Output Format

Generated itineraries include:

1. **Trip Overview** - Budget breakdown and weather forecast
2. **Accommodation** - 3 Airbnb options with prices and details
3. **Transportation** - Options and costs between locations
4. **Day-by-Day Schedule** - With specific timings and distances
5. **Dining Plan** - Recommended restaurants with addresses
6. **Practical Information**:
   - Weather and packing recommendations
   - Currency and costs
   - Local transportation options
   - Safety and cultural tips
   - Communication and health information

## Project Structure

```
ai_travel_planner/
├── app.py                 # Main Streamlit application
├── main.py               # Simple entry point
├── requirements.txt      # Python dependencies
├── pyproject.toml        # Project configuration
└── README.md            # This file
```

## Key Functions

### `run_mcp_travel_planner()`
Async function that:
- Initializes MCP servers for Airbnb and Travel Planner
- Creates an AI travel planner agent
- Generates comprehensive itinerary based on user inputs
- Handles real-time data access

### `generate_ics_content()`
Converts travel plan text to ICS calendar format with:
- Day-by-day events
- Structured descriptions
- Calendar app compatibility

## Troubleshooting

### MCP Connection Issues
- Ensure npm is installed: `npm --version`
- Check internet connection
- Verify API key permissions in Google Cloud Console

### API Key Errors
- Verify API keys are correctly pasted (no extra spaces)
- Check API key permissions and quotas
- For Google Maps, enable required APIs in Cloud Console

### Slow Response Times
- First run may take longer as MCP servers start
- Check internet connection speed
- Consider using local Ollama model for faster responses

### Memory Issues
- If running on low-memory systems, close other applications
- Consider using a smaller Ollama model

## Customization

### Change AI Model
Edit [app.py](app.py) around line 95:

```python
# For local Ollama (default)
model=Ollama(id="qwen2.5:3b")

# For OpenAI via OpenRouter
model=OpenAIChat(id="gpt-4", api_key=openai_key, ...)
```

### Modify Itinerary Detail Level
Edit the agent instructions in `run_mcp_travel_planner()` to adjust:
- Verbosity of recommendations
- Focus areas (food, activities, etc.)
- Cost estimation detail

### Add New Preferences
Edit the quick preference buttons in the Streamlit UI (around line 280) to add custom preference categories.

## Performance Notes

- **First Run**: May take 3-5 minutes as MCP servers initialize
- **Typical Run**: 1-2 minutes per itinerary
- **Model Impact**: Ollama is faster but less detailed; GPT-4 is slower but more comprehensive
- **API Quotas**: Ensure sufficient quota for Google Maps and OpenAI APIs

## Dependencies

See [pyproject.toml](pyproject.toml) for complete list:
- `agno==2.2.10` - AI agent framework
- `streamlit>=1.55.0` - Web UI
- `openai>=2.29.0` - OpenAI API client
- `ollama>=0.6.1` - Local model support
- `icalendar>=7.0.3` - Calendar file generation
- `google-search-results` - Web search capability
- `mcp>=1.26.0` - MCP protocol support

## License

This project is provided as-is for educational and personal use.

## Contributing

Contributions are welcome! Feel free to submit issues and pull requests.

## Support

For issues or questions:
1. Check the Troubleshooting section above
2. Verify all API keys and dependencies are correctly configured
3. Review application logs for error messages
4. Ensure all required services (npm, internet) are available

---

**Happy travels! 🌍✈️**
