# Real-Time YouTube Data Pipeline

A real-time data pipeline that fetches YouTube video statistics from playlists and streams them to Apache Kafka using Confluent Cloud. This project demonstrates how to build a streaming data pipeline that processes YouTube API data in real-time.

## 🚀 Features

- **YouTube API Integration**: Fetches video data from YouTube playlists
- **Real-time Streaming**: Processes and streams data to Apache Kafka
- **Schema Registry**: Uses Confluent Schema Registry for data validation
- **Avro Serialization**: Efficient data serialization with Avro schemas
- **Confluent Cloud**: Cloud-native Kafka streaming platform
- **Configurable**: Easy configuration for different playlists and APIs
- **Telegram Alert Bot**: Integrated to send real-time alerts about YouTube activity

## 📋 Prerequisites

- Python 3.8+
- Confluent Cloud account
- YouTube Data API v3 key
- Virtual environment (recommended)

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd realtime-pipeline
   ```

2. **Create and activate virtual environment**
   ```bash
   python -m venv venv
   # On Windows
   venv\Scripts\activate
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install confluent-kafka[avro]
   pip install requests
   ```

## ⚙️ Configuration

### 1. Set up your configuration file

Edit `scripts/config.py` with your credentials:

```python
config = {
    "google_api_key": "YOUR_YOUTUBE_API_KEY",
    "youtube_playlist_id": "YOUR_PLAYLIST_ID",
    "kafka": {
        "bootstrap.servers": "YOUR_CONFLUENT_CLOUD_BOOTSTRAP_SERVER",
        "security.protocol": "sasl_ssl",
        "sasl.mechanism": "PLAIN",
        "sasl.username": "YOUR_KAFKA_USERNAME",
        "sasl.password": "YOUR_KAFKA_PASSWORD"
    },
    "schema_registry": {
        "url": "YOUR_SCHEMA_REGISTRY_URL",
        "basic.auth.user.info": "YOUR_SCHEMA_REGISTRY_CREDENTIALS"
    }
}
```

### 2. Required Credentials

#### YouTube Data API v3
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable YouTube Data API v3
4. Create credentials (API Key)
5. Add the API key to your config

#### Confluent Cloud
1. Sign up for [Confluent Cloud](https://www.confluent.io/confluent-cloud/)
2. Create a new cluster
3. Get your cluster credentials from the Confluent Cloud console
4. Create a topic named `youtube_videos`
5. Set up Schema Registry and create the `youtube_videos-value` schema

## 🎯 Usage

### Running the Pipeline

```bash
cd scripts
python api_processing.py
```

### What the Pipeline Does

1. **Fetches Playlist Data**: Retrieves all videos from the specified YouTube playlist
2. **Extracts Video Statistics**: For each video, gets:
   - Video ID
   - Title
   - View count
   - Like count
   - Comment count
3. **Serializes Data**: Converts data to Avro format using Schema Registry
4. **Streams to Kafka**: Sends data to the `youtube_videos` topic

### Expected Output

The pipeline will log each video processed:
```
INFO:root:GOT {'kind': 'youtube#video', 'etag': '...', 'id': '...', 'snippet': {...}, 'statistics': {...}}
```

## 📊 Data Schema

The pipeline produces data with the following structure:

```json
{
  "TITLE": "Video Title",
  "VIEWS": 12345,
  "LIKES": 678,
  "COMMENTS": 90
}
```

## 🔧 Project Structure

```
realtime-pipeline/
├── README.md
├── scripts/
│   ├── api_processing.py    # Main pipeline script
│   ├── config.py           # Configuration file
│   ├── api_key             # API key file (optional)
│   └── ksql_code           # KSQL queries (optional)
└── venv/                   # Virtual environment
```

## 🚨 Security Notes

⚠️ **Important**: Never commit sensitive credentials to version control!

- Add `config.py` to your `.gitignore` file
- Use environment variables for production deployments
- Rotate API keys regularly
- Use least-privilege access for all services

## 🔍 Troubleshooting

### Common Issues

1. **Import Errors**: Make sure you've installed `confluent-kafka[avro]`
2. **Authentication Errors**: Verify your Confluent Cloud credentials
3. **Schema Registry Errors**: Ensure the `youtube_videos-value` schema exists
4. **YouTube API Quotas**: Check your YouTube API quota limits

### Debug Mode

Enable debug logging by modifying the logging level in `api_processing.py`:

```python
logging.basicConfig(level=logging.DEBUG)
```

## 📈 Monitoring

Monitor your pipeline using:
- Confluent Cloud Console for Kafka metrics
- YouTube API quota usage
- Application logs for processing status

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 🙏 Acknowledgments

- YouTube Data API v3
- Confluent Cloud
- Apache Kafka
- Confluent Schema Registry

## 📞 Support

For issues and questions:
- Check the troubleshooting section
- Review Confluent Cloud documentation
- Check YouTube API documentation

## 🤖 Telegram Alert Bot Integration

A Telegram bot is integrated to send real-time alerts about YouTube activity (such as changes in likes) directly to your Telegram chat.

### Example Alert Screenshot

![Telegram Alert Example](docs/telegram_alert_example.jpg)

---

**Happy Streaming! 🎬📊** 
