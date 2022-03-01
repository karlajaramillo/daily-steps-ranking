# Activity Meter App

## Description
The Daily Steps Ranking is a web application that provides users with information about their workouts, using the data of the API described below to fetch data.

As a user you can:
- See the overall daily average steps for all the users in the app.
- Sort users in ascending and descending order by overall daily average steps, average steps for last month and last week. 
- See user details and stats about steps, calories, distance and active minutes through a bar chart. Also, see all the activity through a heatmap.


## Technologies
- Vue.js
- Axios
- Netlify
- Pure CSS

## Demo 
![ezgif com-gif-maker (2)](https://user-images.githubusercontent.com/8181053/155973736-445f5eac-6a79-4afc-9f8a-f38b93322ab9.gif)

## Live Version

This app is deployed in Netlify. 
Check it out at: [Activity Meter App](https://activity-meter-karla-jaramillo.netlify.app/).

## API Backend
This API only allows authenticated users by token.

API documentation: https://step-meter-pp4publmdq-ez.a.run.app/docs/

API URL: https://step-meter-pp4publmdq-ez.a.run.app

## Frontend Endpoints 

|	Path	|	Description	|
|	-	|	-	|	
|	/	|	Render home page	|
|	/ranking	|	Render all the users listed	|
|	/userId/:id	|	Render the details for a specific user |

## Issues
- The API returns `next` and `previous` links as `http` URLs, where they should use `https`. To work around this, I switched the `next` URLs to `https`.

## Further improvements for the App
- Instead of fetching data from the API using 'next', I can make only one request using the limit of 100 to make it more efficient: http://step-meter-pp4publmdq-ez.a.run.app/users/?limit=100
- Implement a reusable Pagination component as a solution to separate pages for the results returned by the API, separating them into pages. This will allow the user to view data page by page. Page: /ranking 
- Group workout data in graphs by week and month. Page: /userId/:id

## Frontend - Directory -> daily-steps-ranking

```
npm install
```

## Compile and hot-reload for development

```
npm run serve
```

## Compile and minify for production

```
npm run build
```

## Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
