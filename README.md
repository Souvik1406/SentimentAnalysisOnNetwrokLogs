# SentimentAnalysisOnNetwrokLogs

You can use sentiment analysis on network logs from Azure Sentinel. Sentiment analysis typically applies to text data, so in the context of network logs, you would analyze textual content like alerts, messages, or any descriptive fields to determine the sentiment. This might help in identifying patterns or anomalies, such as unusually negative or positive tones in logs, which could correlate with specific incidents or behaviors.

Here's a basic example of how you could approach this in a Jupyter notebook using Python. This code assumes you have logs in a DataFrame format with at least one column containing text data (e.g., "message") that you want to analyze.

### Prerequisites:
1. Azure Sentinel setup to export or fetch logs into a Pandas DataFrame.
2. Install necessary Python libraries: `pandas`, `nltk`, `textblob`.

### Notebook Code Example

``` 
# Install necessary packages
!pip install pandas nltk textblob

import pandas as pd
from textblob import TextBlob
import nltk

# Download the required NLTK data
nltk.download('punkt')

# Sample DataFrame (Replace this with your actual data)
data = {
    'timestamp': ['2024-08-21 10:00:00', '2024-08-21 10:05:00', '2024-08-21 10:10:00'],
    'message': ['Failed login attempt from IP 192.168.1.1', 
                'User admin successfully logged in from IP 10.0.0.5', 
                'Multiple failed login attempts detected']
}

df = pd.DataFrame(data)

# Function to perform sentiment analysis
def analyze_sentiment(text):
    blob = TextBlob(text)
    sentiment = blob.sentiment
    return sentiment.polarity, sentiment.subjectivity

# Apply sentiment analysis to the 'message' column
df[['polarity', 'subjectivity']] = df['message'].apply(lambda x: pd.Series(analyze_sentiment(x)))

# Display the DataFrame with sentiment analysis results
print(df)

# Example output:
#            timestamp                                       message  polarity  subjectivity
# 0  2024-08-21 10:00:00          Failed login attempt from IP 192.168.1.1      -0.25           0.5
# 1  2024-08-21 10:05:00         User admin successfully logged in from IP 10.0.0.5       0.8           0.75
# 2  2024-08-21 10:10:00         Multiple failed login attempts detected                -0.5           0.6

# You can further analyze or visualize the sentiment scores as needed.
```

### Explanation:
- **Polarity**: Ranges from -1 (negative) to 1 (positive). A negative score indicates negative sentiment, and a positive score indicates positive sentiment.
- **Subjectivity**: Ranges from 0 (objective) to 1 (subjective). A higher score means the text is more subjective.

### Next Steps:
- Integrate this sentiment analysis with your larger security operations workflows.
- Visualize the sentiment analysis results over time to identify trends.
