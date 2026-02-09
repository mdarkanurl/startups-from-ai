# Startups From AI

An automated AI-powered system that discovers, analyzes, and creates content about startups from various online sources. The application continuously crawls the web for startup information, generates AI-powered summaries, and automatically posts engaging content to social media platforms.

## Features

- **Automated Web Crawling** - Continuously discovers startup information from various online sources
- **AI-Powered Analysis** - Uses Google Gemini AI to generate intelligent summaries and insights
- **Multi-Platform Content Generation** - Automatically creates tweets and blog posts about startups
- **Social Media Integration** - Posts generated content to Twitter and Dev.to
- **Data Aggregation** - Collects startup data from Product Hunt, websites, and other sources
- **Scheduled Operations** - Runs automated workflows with different intervals for various tasks
- **Structured Logging** - Comprehensive logging with Winston and Better Stack integration

## Tech Stack

- **Runtime**: Node.js with TypeScript
- **Database**: PostgreSQL with Drizzle ORM + MongoDB for additional storage
- **AI Integration**: Google Gemini API
- **Web Crawling**: Crawlee with Playwright
- **Social APIs**: Twitter API v2, Dev.to API, Product Hunt API
- **Logging**: Winston with daily rotation and Better Stack integration
- **Task Scheduling**: Custom timing system with configurable delays

## Project Structure

```
src/
├── modules/
│   ├── ai/                     # AI-powered content generation
│   │   ├── startups/           # Startup analysis and summarization
│   │   ├── tweet/              # Tweet generation and posting
│   │   └── blog/               # Blog generation and posting
│   └── fetch-data-from-online/ # Data collection modules
│       ├── product-hunt/       # Product Hunt integration
│       └── website-crawler/    # Web scraping functionality
├── db/                         # Database configurations
├── utils/                      # Shared utilities and helpers
├── connection.ts               # Database connection setup
├── winston.ts                  # Logging configuration
└── index.ts                    # Application entry point
```

## Data Models

### Startup
```typescript
interface Startup {
  id: string;
  name?: string;
  VC_firm: string;
  website: string;
  founder_names: string[];
  foundedAt?: string;
}
```

### AI Generated Summary
```typescript
interface AIGeneratedSummary {
  id: string;
  summary: string[];
  startupId: string;
  isUsedForTweets: boolean;
  isUsedForBlogs: boolean;
}
```

### Tweet
```typescript
interface Tweet {
  id: string;
  startupId: string;
  tweet: string;
  isUsed: boolean;
}
```

### Blog
```typescript
interface Blog {
  id: string;
  startupId: string;
  title: string;
  blog: string;
  isUsed: boolean;
}
```

### Web Page Data
```typescript
interface WebPageData {
  id: string;
  url: string;
  title: string;
  description: string;
  text: string;
  isUsed: boolean;
  startupId: string;
}
```

## Setup Instructions

### Prerequisites
- Node.js (v18 or higher)
- pnpm
- PostgreSQL
- MongoDB

### Installation

1. **Install dependencies**
   ```bash
   pnpm install --frozen-lockfile
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` with your configuration:
   - `GEMINI_API_KEY`: Your Google Gemini API key
   - `MONGODB_CONNECT_URL`: MongoDB connection string
   - `DATABASE_URL`: PostgreSQL connection string
   - `X_*`: Twitter API credentials
   - `DEVTO_API_KEY`: Dev.to API key
   - `BEARER_TOKEN`: Product Hunt API token
   - `BETTER_STACK_*`: Better Stack logging configuration

3. **Run database migrations**
   ```bash
   pnpm db:migrate
   ```

4. **Start the application**
   ```bash
   # Development
   pnpm run dev

   # Production
   pnpm run build
   pnpm run start
   ```

## How It Works

### Main Workflow Loop

The application runs in a continuous loop with the following schedule:

1. **Every Loop Iteration**:
   - Crawl websites for startup data
   - Generate AI summaries of startups
   - Generate tweet content
   - Generate blog content

2. **Every Hour**:
   - Post generated tweets to Twitter

3. **Every Day**:
   - Post generated blogs to Dev.to
   - Fetch fresh data from Product Hunt

### Data Flow

1. **Data Collection**: 
   - Product Hunt API for trending startups
   - Web crawler for detailed startup information
   - Website content extraction and analysis

2. **AI Processing**:
   - Google Gemini analyzes collected data
   - Generates comprehensive summaries
   - Creates engaging social media content

3. **Content Distribution**:
   - Automated posting to Twitter
   - Blog publication on Dev.to
   - Tracking of used content to prevent duplicates

## API Integrations

### Product Hunt
- Fetches daily and trending startup data
- Requires API bearer token for authentication

### Twitter/X
- Posts generated tweets automatically
- Uses OAuth 1.0a authentication
- Supports media attachments and threading

### Dev.to
- Publishes comprehensive blog posts
- API key authentication
- Markdown formatting support

### Google Gemini
- Powers content generation and analysis
- Provides intelligent summaries
- Creates engaging social media copy

## Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `GEMINI_API_KEY` | Google Gemini API key | Yes |
| `MONGODB_CONNECT_URL` | MongoDB connection string | Yes |
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `X_APP_KEY` | Twitter app key | Yes |
| `X_APP_SECRET` | Twitter app secret | Yes |
| `X_ACCESS_TOKEN` | Twitter access token | Yes |
| `X_ACCESS_TOKEN_SECRET` | Twitter access token secret | Yes |
| `DEVTO_API_KEY` | Dev.to API key | Yes |
| `BEARER_TOKEN` | Product Hunt bearer token | Yes |
| `HEADLESS` | Run browser in headless mode | No |
| `MAX_REQUESTS` | Maximum requests per crawling session | No |
| `DELAY_MS` | Delay between API requests | No |

### Logging

The application uses Winston for structured logging with:
- Daily log rotation
- Better Stack integration for centralized monitoring
- Different log levels for various components
- Child loggers for better traceability

## Development

### Database Management

```bash
# Generate new migrations
pnpm db:generate

# Run migrations
pnpm db:migrate

# Open database studio
pnpm studio
```

### Monitoring

- Check logs in the console or log files
- Monitor Better Stack dashboard for centralized logging
- Track API usage and rate limits
- Monitor database performance and connections

## Contributing

Contributions, issues, and feature requests are welcome! Please follow the guidelines outlined in the [contributing.md](contributing.md) file.

## License

MIT License

## Support

For questions or support, please open an issue on the GitHub repository.
