This repository contains a two-stage web scraping script for extracting cricket match summaries from ESPN Cricinfo. The script navigates through the website, collects URLs of match summaries, and then parses detailed batting statistics for each match.

### Overview

### Stage 1: Collect Match Summary Links

a.Interaction Code: Navigates to the tournament page and retrieves links to individual match summaries.

b. Parser Code: Parses the HTML to extract URLs of match summary pages.

### Stage 2: Parse Match Details

a. Interaction Code: Navigates to each match summary URL and collects detailed batting statistics.

b. Parser Code: Parses the match summary page to extract and format batting data for both innings.

### Prerequisites

a. JavaScript environment (e.g., Node.js)

b. jQuery library for DOM manipulation

### Usage

### Stage 1: Collect Match Summary Links

### Interaction Code

 
### Interaction Code

  

### Parser Code

 

### Stage 2: Parse Match Details

### Interaction Code

  

### Parser Code
### Output

Stage 1 Output: A list of match summary URLs.

Stage 2 Output: A detailed batting summary for each match, including player names, dismissals, runs, balls faced, 4s, 6s, and strike rates.

### Notes

Ensure the URL in Stage 1 is correctly pointing to the desired tournament page.

The selectors used in the code are specific to ESPN Cricinfo's website structure and may need to be updated if the website changes.
