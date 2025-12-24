# Voter App

A simple web-based voting application built with HTML, CSS, and JavaScript. Users can assign points to candidates and submit their votes, which are stored in Google Sheets via Google Apps Script.

## Features

- Interactive voting table with checkboxes for assigning points (1-9 per candidate).
- Real-time total calculation for each candidate and overall points assigned.
- Validation to ensure at least one point is assigned to each candidate.
- Submission to Google Sheets with detailed vote data.
- Responsive design for mobile and desktop.

## Setup Instructions

1. **Clone or Download the Repository:**
   ```
   git clone https://github.com/yourusername/voter-app.git
   cd voter-app
   ```

2. **Set Up Google Apps Script:**
   - Create a new Google Sheet.
   - Go to Extensions > Apps Script.
   - Replace the default code with the following:

     ```javascript
     function doPost(e) {
       try {
         const data = JSON.parse(e.postData.contents);
         const sheet = SpreadsheetApp.getActiveSheet();
         
         // If sheet is empty, add headers like the HTML table
         if (sheet.getLastRow() === 0) {
           const headers = ['No', 'Name', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'Total'];
           sheet.appendRow(headers);
         }
         
         // Append a row for each candidate
         const candidateNames = Object.keys(data);
         candidateNames.forEach((name, index) => {
           const candidateData = data[name];
           const row = [index + 1, name];
           
           // Add 1 or 0 for each point 1-9
           for (let p = 1; p <= 9; p++) {
             row.push(candidateData.checked.includes(p) ? 1 : 0);
           }
           
           // Add total
           row.push(candidateData.total);
           
           sheet.appendRow(row);
         });
         
         return ContentService
           .createTextOutput('Success')
           .setMimeType(ContentService.MimeType.TEXT);
       } catch (error) {
         return ContentService
           .createTextOutput('Error: ' + error.message)
           .setMimeType(ContentService.MimeType.TEXT);
       }
     }
     ```

   - Save and deploy as a web app (Execute as: Me, Who has access: Anyone).
   - Copy the deployment URL.

3. **Update the HTML File:**
   - Open `index.html`.
   - Replace `'YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL'` with your actual Apps Script URL.

4. **Run the Application:**
   - Open `index.html` in a web browser.
   - Assign points to candidates and submit.

## Technologies Used

- HTML5
- CSS3
- JavaScript (ES6)
- Google Apps Script
- Google Sheets API

## Contributing

Feel free to fork the repository and submit pull requests for improvements.

## License

This project is open-source. Please check the license file for details.