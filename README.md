<div align="center">

# 📊 YouTube Data Analytics: End-to-End Pipeline

**YouTube Data API → Python → MySQL → Power BI**

*Analyzing the top data-analytics YouTubers: who has the reach, the views and the engagement?*

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![YouTube API](https://img.shields.io/badge/YouTube_Data_API_v3-FF0000?style=for-the-badge&logo=youtube&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)

</div>

---

## 📌 Overview

This project is a complete data analytics workflow built around YouTube channels that teach **data analytics, data science and Power BI**. Channel and video statistics are pulled from the **YouTube Data API v3** using Python, stored in **MySQL**, explored with advanced SQL (CTEs, window functions, stored procedures), and finally visualized in an interactive **Power BI** dashboard with a custom YouTube-themed colour palette.


## 🏗️ Project Architecture

```
┌──────────────────┐    ┌─────────────┐    ┌───────────────┐    ┌───────────────┐
│ YouTube Data API │ ─▶ │   Python    │ ─▶ │     MySQL     │ ─▶ │   Power BI    │
│       (v3)       │    │ pandas, API │    │ EDA + Procs   │    │   Dashboard   │
└──────────────────┘    └─────────────┘    └───────────────┘    └───────────────┘
   Channel & video       Clean, convert     Store, validate,     Interactive KPIs,
   statistics            and load data      query and rank       charts, slicers
```

## 🛠️ Tech Stack

| Layer | Tools |
|-------|-------|
| Data Extraction | YouTube Data API v3, `google-api-python-client` |
| Data Processing | Python, Pandas, Seaborn |
| Database | MySQL, `mysql-connector-python`, SQLAlchemy |
| Analysis | SQL (CTEs, window functions, views, stored procedures) |
| Visualization | Power BI Desktop, custom JSON theme |

## 📁 Repository Structure

```
youtube-data-pipeline-eda-powerbi/
├── youtube-data-pipeline.ipynb      # Python: API extraction, cleaning, loading to MySQL
├── youtube-data-pipeline.pdf        # PDF export of the notebook
├── EDAYouTubeChannels.sql           # MySQL: schema, EDA queries, stored procedures
├── YoutubeAnalyticsDashboard.pbix   # Power BI dashboard
├── youtube_red.json                 # Custom Power BI theme (YouTube maroon/red)
└── README.md
```

## 🔄 Workflow

### 1️⃣ Data Extraction (Python)
- Connected to the YouTube Data API v3 using `googleapiclient`.
- Fetched **channel-level stats** (subscribers, total videos, total views) for multiple creators in a single API call.
- Retrieved each channel's **uploads playlist ID** and paginated through it to collect every video ID.
- Fetched **video-level stats** (title, published date, views, likes, comments) in batches of 50.
- Converted data types with Pandas and loaded the results into MySQL using SQLAlchemy.

### 2️⃣ Exploratory Data Analysis (MySQL)
- Created the `youtube_analytics` database with `youtube_channel_data` and `videos_data` tables.
- Validated value ranges (min / max / zero-value checks) on views, likes and comments.
- Ranked channels and videos using `DENSE_RANK()` window functions and CTEs.
- Built reusable **stored procedures**:

| Procedure | Purpose |
|-----------|---------|
| `channelssummary(channel_name)` | Returns full details of a channel, with validation for invalid names |
| `channeldetailsbysubscribersrank(rank)` | Returns the channel at a given subscriber rank |
| `videosrankbychannels(channel_name)` | Returns the top 10 most viewed videos of a channel |

- Created a **view** for the most-viewed video of a channel and analysed uploads and views by year and month.

### 3️⃣ Dashboarding (Power BI)
Two interactive report pages with a custom dark-red YouTube theme:

**Page 1: Channel Overview**
- Subscribers by channel (bar chart)
- Total videos by channel (bar chart)
- Views share by channel (pie chart)
- Detailed table with conditional formatting

**Page 2: Video Performance (Top 5 channels by reach)**
- KPI cards: Total Videos, Total Views, Total Likes, Total Comments
- Channel slicer for interactive filtering
- Upload and views trends by year, quarter and month
- Word cloud of video titles
- Top videos by views

## 📈 Key Insights

- 🏆 **codebasics** leads in subscribers (**914K**), followed by **Krish Naik** (859K) and **Alex The Analyst** (657K).
- 👀 **Chandoo** has the highest channel views (**~43.5M**) among the channels in the overview page.
- 🎬 **Krish Naik** has uploaded the most videos (**1,771**), nearly 2.3x more than codebasics (772).
- 📊 Among the top 5 channels, **Krish Naik (87M)** and **codebasics (65M)** account for over **57%** of total views.
- ⚡ **techTFQ** has only 100 videos yet crosses **13M views**, which makes it the most views-per-video efficient channel in the group.
- 🔤 Video titles are dominated by keywords like *Data*, *Python*, *Tutorial* and *Machine Learning*.

## 🖼️ Dashboard Preview

> 📷 *Add screenshots to an `images/` folder and update the paths below.*

| Channel Overview | Video Performance |
|:---:|:---:|
| ![Overview](images/dashboard-page1.png) | ![Videos](images/dashboard-page2.png) |

## 🚀 How to Run

### Prerequisites
- Python 3.8+
- MySQL Server
- Power BI Desktop
- A YouTube Data API key ([get one here](https://console.cloud.google.com/apis/library/youtube.googleapis.com))

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/Nameis-nilesh/youtube-data-pipeline-eda-powerbi.git
   cd youtube-data-pipeline-eda-powerbi
   ```

2. **Install dependencies**
   ```bash
   pip install google-api-python-client pandas seaborn mysql-connector-python sqlalchemy jupyter
   ```

3. **Set up MySQL**: run the table creation section of `EDAYouTubeChannels.sql`.

4. **Add your credentials** in the notebook (never commit real keys):
   ```python
   api_key = "YOUR_API_KEY"
   # MySQL: user, password, host, database = youtube_analytics
   ```

5. **Run the notebook** `youtube-data-pipeline.ipynb` to extract data and load it into MySQL.

6. **Run the remaining queries** in `EDAYouTubeChannels.sql` for the EDA and stored procedures.

7. **Open the dashboard**: open `YoutubeAnalyticsDashboard.pbix` in Power BI Desktop and refresh the data source.  
   To apply the theme: *View → Themes → Browse for themes →* `youtube_red.json`.

## 🔐 Security Note

API keys and database passwords are **not** included in this repository. Replace the placeholders with your own credentials, preferably through environment variables.



## 👤 Author

**Nilesh Yadav**

[![GitHub](https://img.shields.io/badge/GitHub-Nameis--nilesh-181717?style=flat&logo=github)](https://github.com/Nameis-nilesh)

---

<div align="center">

⭐ If you found this project useful, consider giving it a star!

</div>
