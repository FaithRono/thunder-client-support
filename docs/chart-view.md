# Chart View (Beta)
- The feature is useful for Response Data Visualisation
- Create charts or tables from response using `tc.chartHTML()` from the Tests tab scripting
- When you pass data to function `tc.chartHTML(template, data)`, the data is available in `chart_data` global variable
- The feature is in Beta, please test and let us know feedback

```js
var template = `
<script src="https://cdnjs.cloudflare.com/ajax/libs/handlebars.js/4.7.8/handlebars.min.js"></script>

    <div id="output"></div>
    <script id="entry-template" type="text/x-handlebars-template">
          <div class="entry">
            <h2>{{first_name}}</h2>
            <div class="body">
              {{email}}
            </div>
          </div>
        </script>
    <script>
    
    var source = document.getElementById("entry-template").innerHTML;
    var template = Handlebars.compile(source);

    document.getElementById("output").innerHTML = template(chart_data[0]);
    </script>
`;

var data = tc.response.json.data;
tc.chartHTML(template, data);
```


### Convert JSON Response to Html Table
Here is generic example below to convert any response `JSON array` to HTML Table

You can modify the style as needed


```js


var response = tc.response.json;

var html = `
    <style>
      body {
        font-size: 13px;
        font-family: "HelveticaNeue", "Helvetica Neue", Helvetica, Arial, sans-serif;
      }

      table {
        width: 100%;
        box-sizing: border-box;
        border-collapse: collapse;
        border-spacing: 0;
      }

      td,
      th {
        padding: 0;
      }

      th, td {
        padding: 4px 6px;
        text-align: left;
        border-bottom: 1px solid #E1E1E1;
      }

      th:first-child,
      td:first-child {
        padding-left: 0;
      }

      th:last-child,
      td:last-child {
        padding-right: 0;
      }

     /* for dark mode use .vscode-dark to override */
     .vscode-dark th, .vscode-dark td {
        border-bottom: 1px solid #555;
      }
  
  </style>

    <div id="container"></div>
    
    <script>
         // get the container element
         let container = document.getElementById("container");
         
         var cols = Object.keys(chart_data[0]);

          var headerRow = '<tr>';
          var bodyRows = '';
      
          cols.map(function(col) {
              headerRow += '<th>' + col + '</th>';
          });
      
          headerRow += '</tr>';
      
          chart_data.map(function(row) {
              bodyRows += '<tr>';
      
              cols.map(function(colName) {
                  bodyRows += '<td>' + row[colName] + '</td>';
              })
      
              bodyRows += '</tr>';
          });
      
          var tabledata=  '<table>' + headerRow + bodyRows + '</table>';
         container.innerHTML = tabledata; // set table html
      
   </script>`
   
   tc.chartHTML(html, response);
```

### Dark Mode Stylew

- For dark mode you `.vscode-dark` to override styles, see example below
```css
    /* for dark mode use .vscode-dark to override */
     .vscode-dark th, .vscode-dark td {
        border-bottom: 1px solid #555;
      }
```
