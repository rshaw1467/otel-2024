## Metric Visualization & Analysis

Now that we have our metrics in Dynatrace we need to create a business & observability dashboard. 

---

## Dashboarding

#### 📌 1 - Clone `Daily Sales - Business Dashbaord`

Open `Daily Sales - Business Dashbaord` from Dashboards and clone it, we'll modify this to fit your specific metrics.

Rename the Cloned dashboard to include your name in it. 

#### 📌 2 - Modify the Business tiles [1 or more]

Modify the business tiles so that they use your specific metrics. 

For `Total Sales Revenue`:
1. Click the tile
1. Click `Configure in Data Explorer`
1. Paste your metric `shop.<NAME>.revenue.actual.count`
1. Click `Run Query`
1. Click `Save Changes to Dashboard`

Repeate these steps for the other tiles (not expected to finsih this task)

#### 📌 3 - Modify the Observability tiles [1 or more]

Modify the business tiles so that they use your specific metrics. 

For `Response Time`:
1. Click the tile
1. Click `Configure in Data Explorer`
1. Change the Filter to your service `Service` > `Name` > `<NAME>-backend-api`
1. Click `Run Query`
1. Click `Save Changes to Dashboard`

#### 📌 4 - Add a tile to display the response time of the top 10 endpoints in the backend-api service

1. Drag a Top List tile into the dashboard.
1. Click the tile and select `Configure tile in Data Explorer`
2. Select the `Unified service request response time (by service, endpoint)`
1. Split by `Endpoint`
1. Click `+` and select `Limit` and select the value of `10`
1. Click `↻ Run Query`
1. Save changes to dashboard
1. Rename the tile to `Endpoint Response Time`