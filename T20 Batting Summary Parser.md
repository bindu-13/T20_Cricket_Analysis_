This repository contains a two-stage web scraping script for extracting cricket match summaries from ESPN Cricinfo. The script navigates through the website, collects URLs of match summaries, and then parses detailed batting statistics for each match.

**Overview**
**Stage 1: Collect Match Summary Links**
1.a Interaction Code: Navigates to the tournament page and retrieves links to individual match summaries.
1.b Parser Code: Parses the HTML to extract URLs of match summary pages.
**Stage 2: Parse Match Details**
2.a Interaction Code: Navigates to each match summary URL and collects detailed batting statistics.
2.b Parser Code: Parses the match summary page to extract and format batting data for both innings.
**Prerequisites**
JavaScript environment (e.g., Node.js)
jQuery library for DOM manipulation
Usage
Stage 1: Collect Match Summary Links
// Interaction Code
navigate('https://stats.espncricinfo.com/ci/engine/records/team/match_results.html?id=14450;type=tournament');

let links = parse().matchSummaryLinks;
for(let i of links) { 
  next_stage({url: i}) 
}

// Parser Code
let links = []
const allRows = $('table.engineTable > tbody > tr.data1');
allRows.each((index, element) => {
 const tds = $(element).find('td');
 const rowURL = "https://www.espncricinfo.com" +$(tds[6]).find('a').attr('href');
 links.push(rowURL);
})
return {
  'matchSummaryLinks': links
};
Stage 2: Parse Match Details
// Interaction Code
navigate(input.url);
collect(parse());

// Parser Code
var match = $('div').filter(function(){
  return $(this)
    .find('span > span > span').text() === String("Match Details") 
}).siblings()
team1 = $(match.eq(0)).find('span > span > span').text().replace(" Innings", "")
team2 = $(match.eq(1)).find('span > span > span').text().replace(" Innings", "")
matchInfo = team1 +  ' Vs ' + team2

var tables = $('div > table.ci-scorecard-table');
var firstInningRows = $(tables.eq(0)).find('tbody > tr').filter(function(index, element){
  return $(this).find("td").length >= 8
})

var secondInningsRows = $(tables.eq(1)).find('tbody > tr').filter(function(index, element){
  return $(this).find("td").length >= 8
});

var battingSummary = []
firstInningRows.each((index, element) => {
  var tds = $(element).find('td');
  battingSummary.push({
    "match": matchInfo,
    "teamInnings": team1,
    "battingPos": index+1,
    "batsmanName": $(tds.eq(0)).find('a > span > span').text().replace(' ', ''),
    "dismissal": $(tds.eq(1)).find('span > span').text(),
    "runs": $(tds.eq(2)).find('strong').text(), 
    "balls": $(tds.eq(3)).text(),
    "4s": $(tds.eq(5)).text(),
    "6s": $(tds.eq(6)).text(),
    "SR": $(tds.eq(7)).text()
  });
});

secondInningsRows.each((index, element) => {
  var tds = $(element).find('td');
  battingSummary.push({
    "match": matchInfo,
    "teamInnings": team2,
    "battingPos": index+1,
    "batsmanName": $(tds.eq(0)).find('a > span > span').text().replace(' ', ''),
    "dismissal": $(tds.eq(1)).find('span > span').text(),
    "runs": $(tds.eq(2)).find('strong').text(), 
    "balls": $(tds.eq(3)).text(),
    "4s": $(tds.eq(5)).text(),
    "6s": $(tds.eq(6)).text(),
    "SR": $(tds.eq(7)).text()
  });
});

return {"battingSummary": battingSummary}
**Output**
Stage 1 Output: A list of match summary URLs.
Stage 2 Output: A detailed batting summary for each match, including player names, dismissals, runs, balls faced, 4s, 6s, and strike rates.
**Notes**
Ensure the URL in Stage 1 is correctly pointing to the desired tournament page.
The selectors used in the code are specific to ESPN Cricinfo's website structure and may need to be updated if the website changes.
