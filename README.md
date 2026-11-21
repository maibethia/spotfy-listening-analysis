# Spotify Listening Analysis: what makes a listener skip?

**[Interactive dashboard on Tableau Public](LINK-HERE)** · **[Notebook](spotify_analysis.ipynb)**

![Dashboard screenshot](images/dashboard.png)

## The question
How does a listener actually use Spotify: when do they listen, what do they finish versus abandon, and do newly discovered artists stick? Can a model predict a skip before it happens?

Skip rate and retention are core metrics for any streaming service, so I approached this the way a product analyst would.

## Data
- **Version 1:** a public Spotify streaming history dataset from Maven Analytics (one real user's history, X rows, DATE to DATE).
- **Version 2 (in progress):** my own Spotify Extended Streaming History, using the same pipeline.

Key fields: timestamp, track, artist, album, milliseconds played, platform, shuffle, and the reason each track started and ended.

## Cleaning
<!-- Paste the output of the "Cleaning log" cell here, then add a sentence on WHY for each -->
- Started with X rows, finished with Y.
- Removed X rows with no track or artist name (podcasts and Spotify's known missing-metadata rows).
- ...
- Removed personal fields (IP address, username) before analysis and publication.

## Definitions
| Metric | Definition | Why |
|---|---|---|
| Stream | Played for 30 seconds or more | Spotify's own threshold for counting a stream |
| Skip | `skipped` flag is true, or the track ended via the forward button | The flag alone was blank for some rows |
| Session | Plays with no gap longer than 30 minutes | Approximates one sitting of listening |
| 90-day retention | An artist still played 90+ days after first listen | Measures whether discovery "sticks" |
| Artist skip rate | Only artists with 20+ plays | Avoids misleading rates from tiny samples |

## Key findings
1. **Insight.** So what.
2. **Insight.** So what.
3. **Insight.** So what.

## Predicting skips
- **Goal:** predict whether a track will be skipped using only information known when it starts.
- **Avoiding leakage:** excluded `ms_played` and `reason_end`, since they reveal the answer.
- **Train/test split:** trained on the earliest 80% of plays and tested on the latest 20%, so the model predicts the future from the past.
- **Results:** Logistic regression ROC AUC X; random forest ROC AUC Y. Skip recall X%, precision Y%.
- **Top drivers of skipping:** ...
- **Why not accuracy:** a model that always guesses the most common outcome already scores X%, so I focused on precision, recall and ROC AUC.

## How I used AI
- Used AI assistance (GitHub Copilot / an AI assistant) to speed up boilerplate code and debugging.
- Wrote the metric definitions and interpretation myself, and checked every output against the raw data.
- Stripped personal fields before sharing any data with AI tools.

## Limitations
- One listener's data, so the findings describe one person, not Spotify users in general.
- Some `skipped` values were blank and were treated as "not skipped".
- No genre or track-length data in the export.

## Next steps
- Rerun on my own extended history and compare.
- Add genre via a music metadata API.
- Try features such as time since last play of that artist.

## Run it yourself
```
python -m venv .venv
# Windows: .venv\Scripts\activate   |   Mac: source .venv/bin/activate
pip install -r requirements.txt
```
Put the CSV in `data/`, open `spotify_analysis.ipynb` in VS Code and run all cells.

**Tools:** Python (pandas, scikit-learn, seaborn), Tableau Public, VS Code, Git
