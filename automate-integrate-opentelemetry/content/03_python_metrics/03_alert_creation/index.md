## Creating alerts in Dynatrace

Here we look at ad-hock anlysis of metrics connected to our python service and creating alerts on our metrics. 

---

## Metric Events (Alerting)

#### 📌 Task

Lets create an Auto Adaptive baseline metric event for the `shop.database.connections.active` metric. 

Navigate to `Settings` > `Anomoly detection` > `Metric Events` > `Add Metric Event`

Set the following:

| Field | Value |
| ------ | ------------- |
| Summary | `Shop: Connections Active`  |
| Type | `Metric Selector` |
| Metric selector | `shop.database.connections.active:splitBy():sort(value(auto,descending)):limit(20)` |

Set the following in the `Monitoring strategy` section:

| Field | Value |
| ------ | ------------- |
| Model Type | `Static threshold`  |
| Number of signal fluctuations | `28` |
| Alert condition | `Alert if metric is above` | 

Then in Alert Preview:

| Field | Value |
| ------ | ------------- |
| Dimension values | `Dimensionless metric`  |

This should trigger a preview based on past data where alerts would have triggered with your current settings.

If you find it too sensitive play with the settings labeled `Advanced model properties` to adjust the alerting, dealerting, and sample period.  Here is a good visualization of how this works:

![Sliding Window](../../../assets/images/03-04-sliding-window-example.jpg)

![Alert Preview](../../../assets/images/03-04-alert_setup.png)

In the `Alert preview` select `Service=pysrvc svc on port 8090` as the Dimension Value to get the alert preview:

![Alert Preview](../../../assets/images/03-04-alert_preview.png)
