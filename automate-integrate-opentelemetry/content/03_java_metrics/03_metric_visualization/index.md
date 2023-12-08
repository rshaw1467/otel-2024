## Metric Visualization & Analysis

Now that we have our metrics in Dynatrace we need to create a business & observability dashboard. 

---

## Dashboarding

#### 📌 1 - Clone `Daily Sales - Business Dashbaord`

Open `Daily Sales - Business Dashbaord` from Dashboards and clone it, we'll modify this to fit your specific metrics.

#### 📌 2 - Modify the Business tiles

Modify the business tiles so that they use your specific metrics. 

<to-do>

#### 📌 3 - Modify the Observability tiles

Modify the observability tiles so that they use your specific metrics. 

<to-do>

#### 📌 4 - Add a tile to display the response time of the top 5 endpoints in the backend service

1. Drag a Top List tile into the dashboard.
1. Click the tile and select `Configure tile in Data Explorer`
2. Select the `Unified service request response time (by service, endpoint)`
1. Split by `Endpoint`
1. Click `+` and select `Limit` and select the value of `10`
1. Click `↻ Run Query`
1. Save changes to dashboard
1. Rename the tile to `Endpoint Response Time`