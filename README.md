<!doctype html>
<html>
<head>
  <title>Assignment 1</title>
  <style>
    
    body {
      font-family: 'Times New Roman', sans-serif; 
      font-size: 16px;  
      color: #9b8baa;      
    }

    h2 {
      font-family: 'Georgia', serif;   
      font-size: 48px;
    color: #a47a97;                  
    }

    h3 {
      font-family: "Comic Sans MS", cursive, sans-serif; 
      font-size: 32px;
    color: #cf61ae;
    }

    h4 {
      font-family: 'Georgia', serif;  
      font-size: 16pxs;
      color: #2c3e50;                  
    }


    table {
      width: 50%;
      margin: 20px auto;
      border-collapse: collapse;
      font-family: 'Verdana', sans-serif;  /* 表格使用特定字体 */
    }

    th, td {
      border: 1px solid black;
      padding: 10px;
      text-align: center;
      font-size: 14px;  /* 更改表格字体大小 */
    }

    th {
      background-color: #f2f2f2;
      font-weight: bold; /* 表格标题字体加粗 */
    }

    #searchInput {
      width: 50%;
      margin: 20px auto;
      padding: 10px;
      display: block;
      text-align: center;
      font-family: 'Courier New', monospace;  /* 搜索框使用不同字体 */
      font-size: 16px;
    }
  </style>
</head>
<body>

<h2 style="text-align:center;">Singapore 2-hr Weather Forecast</h2>
<h3 style="text-align:center;">WANG YIRUN</h3>

<!-- Display last updated and valid to time -->
<h4 id="timestring" style="text-align:center;"></h4>
<h4 id="validTo" style="text-align:center;"></h4>

<!-- Search bar to filter the table -->
<input type="text" id="searchInput" onkeyup="filterTable()" placeholder="Search for area..." />

<!-- Table to display the weather data -->
<table id="weatherTable" align="center">
  <tr>
    <th>Area</th>
    <th>Forecast</th>
  </tr>
</table>

<script>
  // Fetch the 2-hour weather forecast data
  fetch('https://api.data.gov.sg/v1/environment/2-hour-weather-forecast')
    .then(response => response.json())
    .then(responsedata => {

      // Extract the timestamp, valid to, and forecast data
      let timestamp = responsedata.items[0].update_timestamp;
      let validTo = responsedata.items[0].valid_period.end;
      let forecasts = responsedata.items[0].forecasts;

      // Display the last updated time
      let timestring = document.getElementById('timestring');
      timestring.innerHTML = "Last updated at: " + new Date(timestamp).toLocaleString();

      // Display the valid to time
      let validToString = document.getElementById('validTo');
      validToString.innerHTML = "Valid until: " + new Date(validTo).toLocaleString();

      // Reference to the weather table
      let weatherTable = document.getElementById('weatherTable');

      // Loop through each forecast and add a row in the table
      forecasts.forEach(forecast => {
        let row = weatherTable.insertRow();  // Create a new row
        let cell1 = row.insertCell(0);  // Create the first cell (Area)
        let cell2 = row.insertCell(1);  // Create the second cell (Forecast)

        cell1.innerHTML = forecast.area;  // Fill in the area
        cell1.setAttribute('data-area', forecast.area.toLowerCase());  // For search filtering
        cell2.innerHTML = forecast.forecast;  // Fill in the forecast
      });
    })
    .catch(error => {
      console.error('Error fetching weather data:', error);
    });

  // Function to filter table based on search input
  function filterTable() {
    let input = document.getElementById('searchInput').value.toLowerCase();
    let table = document.getElementById('weatherTable');
    let tr = table.getElementsByTagName('tr');

    // Loop through all table rows, and hide those who don't match the search query
    for (let i = 1; i < tr.length; i++) {
      let td = tr[i].getElementsByTagName('td')[0];  // Get the first column (Area)
      if (td) {
        let area = td.getAttribute('data-area');
        if (area.indexOf(input) > -1) {
          tr[i].style.display = '';
        } else {
          tr[i].style.display = 'none';
        }
      }
    }
  }
</script>

</body>
</html>