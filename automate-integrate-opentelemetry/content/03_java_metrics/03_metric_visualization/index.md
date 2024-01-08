## Metric Visualization & Analysis

Now that we have our metrics in Dynatrace we need to create a business & observability dashboard. 

---

## Dashboarding

#### 📌 1 - Upload `Daily Sales - Business Dashbaord`

Copy the following JSON to a file and save it as `Shop Business Health.json`. 

<details>
  <summary>Expand for dashboard JSON</summary>

```JSON
{
  "metadata": {
    "configurationVersions": [
      7
    ],
    "clusterVersion": "1.281.79.20231208-113952"
  },
  "id": "3041c929-2efd-42f6-9843-2b570996329c",
  "dashboardMetadata": {
    "name": "Shop Business Health",
    "shared": false,
    "owner": "ryan.shaw@dynatrace.com",
    "hasConsistentColors": false
  },
  "tiles": [
    {
      "name": "Backend Error Rate",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 912,
        "left": 570,
        "width": 570,
        "height": 342
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "builtin:service.errors.total.rate",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [
            "dt.entity.service"
          ],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "filterOperator": "AND",
            "nestedFilters": [
              {
                "filter": "dt.entity.service",
                "filterType": "NAME",
                "filterOperator": "OR",
                "entityAttribute": "entityName",
                "nestedFilters": [],
                "criteria": [
                  {
                    "value": "ryan-order-backend on port *",
                    "evaluator": "IN",
                    "matchExactly": true
                  }
                ]
              }
            ],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "STACKED_AREA",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "unitTransform": "auto",
            "valueFormat": "auto",
            "properties": {
              "color": "RED",
              "seriesType": "STACKED_AREA"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "displayName": "",
            "visible": true
          },
          "yAxes": [
            {
              "displayName": "",
              "visible": true,
              "min": "AUTO",
              "max": "AUTO",
              "position": "LEFT",
              "queryIds": [
                "A"
              ],
              "defaultAxis": true
            }
          ]
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "color": "#7dc540"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": [
        "resolution=null&(builtin:service.errors.total.rate:filter(and(or(in(\"dt.entity.service\",entitySelector(\"type(service),entityName.equals(~\"ryan-order-backend on port *~\")\"))))):splitBy(\"dt.entity.service\"):sort(value(auto,descending)):limit(20)):limit(100):names"
      ]
    },
    {
      "name": "Service Health",
      "tileType": "HEADER",
      "configured": true,
      "bounds": {
        "top": 874,
        "left": 0,
        "width": 342,
        "height": 38
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false
    },
    {
      "name": "Business Health",
      "tileType": "HEADER",
      "configured": true,
      "bounds": {
        "top": 0,
        "left": 0,
        "width": 304,
        "height": 38
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false
    },
    {
      "name": "Total Sales Revenue",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 38,
        "left": 0,
        "width": 570,
        "height": 380
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "shop.ryan.revenue.actual.count",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "SINGLE_VALUE",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "properties": {
              "color": "DEFAULT"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "visible": true
          },
          "yAxes": []
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "singleValueSettings": {
          "showTrend": true,
          "showSparkLine": true,
          "linkTileColorToThreshold": true
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "value": 0,
                "color": "#7dc540"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "queryId": "",
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": []
    },
    {
      "name": "Number of Purchases",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 38,
        "left": 570,
        "width": 570,
        "height": 380
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "shop.ryan.purchases.confirmed.count",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "SINGLE_VALUE",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "properties": {
              "color": "DEFAULT"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "visible": true
          },
          "yAxes": []
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "singleValueSettings": {
          "showTrend": true,
          "showSparkLine": true,
          "linkTileColorToThreshold": true
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "value": 0,
                "color": "#14a8f5"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "queryId": "",
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": []
    },
    {
      "name": "Average $ Per Purchase",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 38,
        "left": 1140,
        "width": 570,
        "height": 380
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "shop.revenue.actual.count",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": false
        },
        {
          "id": "B",
          "metric": "shop.purchases.confirmed.count",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": false
        },
        {
          "id": "C",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "metricSelector": "shop.ryan.revenue.actual.count:splitBy():sort(value(auto,descending)):limit(20) / shop.ryan.purchases.confirmed.count:splitBy():sort(value(auto,descending)):limit(20)",
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "SINGLE_VALUE",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "unitTransform": "auto",
            "valueFormat": "auto",
            "properties": {
              "color": "DEFAULT",
              "seriesType": "LINE"
            },
            "seriesOverrides": []
          },
          {
            "matcher": "B:",
            "unitTransform": "auto",
            "valueFormat": "auto",
            "properties": {
              "color": "DEFAULT",
              "seriesType": "LINE"
            },
            "seriesOverrides": []
          },
          {
            "matcher": "C:",
            "properties": {
              "color": "DEFAULT"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "visible": true
          },
          "yAxes": []
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "singleValueSettings": {
          "showTrend": true,
          "showSparkLine": true,
          "linkTileColorToThreshold": true
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "value": 0,
                "color": "#9cd575"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "queryId": "",
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": []
    },
    {
      "name": "Top Product by Sales",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 456,
        "left": 0,
        "width": 570,
        "height": 380
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "shop.ryan.purchases.confirmed.count",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [
            "product"
          ],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "TOP_LIST",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "properties": {
              "color": "DEFAULT"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "visible": true
          },
          "yAxes": []
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "color": "#7dc540"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": []
    },
    {
      "name": "Revenue - Expected v Actual",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 456,
        "left": 570,
        "width": 570,
        "height": 380
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "shop.ryan.revenue.actual.count",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        },
        {
          "id": "B",
          "metric": "shop.ryan.revenue.expected.count",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "GRAPH_CHART",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "unitTransform": "auto",
            "valueFormat": "auto",
            "properties": {
              "color": "RED",
              "seriesType": "LINE",
              "alias": "Actual Revenue"
            },
            "seriesOverrides": []
          },
          {
            "matcher": "B:",
            "unitTransform": "auto",
            "valueFormat": "auto",
            "properties": {
              "color": "GREEN",
              "seriesType": "LINE",
              "alias": "Expected Revenue"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "displayName": "",
            "visible": true
          },
          "yAxes": [
            {
              "displayName": "",
              "visible": true,
              "min": "AUTO",
              "max": "AUTO",
              "position": "LEFT",
              "queryIds": [
                "A",
                "B"
              ],
              "defaultAxis": true
            }
          ]
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "color": "#7dc540"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": []
    },
    {
      "name": "Lost Sales (due to error)",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 456,
        "left": 1140,
        "width": 570,
        "height": 380
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "metricSelector": "shop.ryan.revenue.expected.count:splitBy():sort(value(auto,descending)):limit(20) - shop.ryan.revenue.actual.count:splitBy():sort(value(auto,descending)):limit(20)",
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "SINGLE_VALUE",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "properties": {
              "color": "DEFAULT"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "visible": true
          },
          "yAxes": []
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "singleValueSettings": {
          "showTrend": true,
          "showSparkLine": true,
          "linkTileColorToThreshold": true
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "color": "#7dc540"
              },
              {
                "color": "#f5d30f"
              },
              {
                "value": 0,
                "color": "#dc172a"
              }
            ],
            "queryId": "",
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": []
    },
    {
      "name": "Database Active Connections",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 912,
        "left": 1140,
        "width": 570,
        "height": 342
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "shop.database.connections.active",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "nestedFilters": [],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "STACKED_AREA",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "unitTransform": "auto",
            "valueFormat": "auto",
            "properties": {
              "color": "GRAY",
              "seriesType": "STACKED_AREA",
              "alias": "Active Connections"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "displayName": "",
            "visible": true
          },
          "yAxes": [
            {
              "displayName": "",
              "visible": true,
              "min": "AUTO",
              "max": "AUTO",
              "position": "LEFT",
              "queryIds": [
                "A"
              ],
              "defaultAxis": true
            }
          ]
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "color": "#7dc540"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": [
        "resolution=null&(shop.database.connections.active:splitBy():sort(value(auto,descending)):limit(20)):limit(100):names"
      ]
    },
    {
      "name": "Response Time",
      "tileType": "DATA_EXPLORER",
      "configured": true,
      "bounds": {
        "top": 912,
        "left": 0,
        "width": 570,
        "height": 342
      },
      "tileFilter": {},
      "isAutoRefreshDisabled": false,
      "customName": "Data explorer results",
      "queries": [
        {
          "id": "A",
          "metric": "builtin:service.request.response_time_service_aggregation",
          "spaceAggregation": "AUTO",
          "timeAggregation": "DEFAULT",
          "splitBy": [],
          "sortBy": "DESC",
          "sortByDimension": "",
          "filterBy": {
            "filterOperator": "AND",
            "nestedFilters": [
              {
                "filter": "dt.entity.service",
                "filterType": "NAME",
                "filterOperator": "OR",
                "entityAttribute": "entityName",
                "nestedFilters": [],
                "criteria": [
                  {
                    "value": "ryan-order-backend",
                    "evaluator": "IN",
                    "matchExactly": true
                  }
                ]
              }
            ],
            "criteria": []
          },
          "limit": 20,
          "rate": "NONE",
          "enabled": true
        }
      ],
      "visualConfig": {
        "type": "GRAPH_CHART",
        "global": {},
        "rules": [
          {
            "matcher": "A:",
            "properties": {
              "color": "DEFAULT"
            },
            "seriesOverrides": []
          }
        ],
        "axes": {
          "xAxis": {
            "displayName": "",
            "visible": true
          },
          "yAxes": [
            {
              "displayName": "",
              "visible": true,
              "min": "AUTO",
              "max": "AUTO",
              "position": "LEFT",
              "queryIds": [
                "A"
              ],
              "defaultAxis": true
            }
          ]
        },
        "heatmapSettings": {
          "yAxis": "VALUE"
        },
        "thresholds": [
          {
            "axisTarget": "LEFT",
            "rules": [
              {
                "color": "#7dc540"
              },
              {
                "color": "#f5d30f"
              },
              {
                "color": "#dc172a"
              }
            ],
            "visible": true
          }
        ],
        "tableSettings": {
          "hiddenColumns": []
        },
        "graphChartSettings": {
          "connectNulls": false
        },
        "honeycombSettings": {
          "showHive": true,
          "showLegend": true,
          "showLabels": false
        }
      },
      "queriesSettings": {
        "resolution": ""
      },
      "metricExpressions": [
        "resolution=null&(builtin:service.request.response_time_service_aggregation:filter(and(or(in(\"dt.entity.service\",entitySelector(\"type(service),entityName.equals(~\"ryan-order-backend~\")\"))))):splitBy():sort(value(auto,descending)):limit(20)):limit(100):names"
      ]
    }
  ]
}
```
</details>

Once saved upload the dashboard to your tenant - open `Dashboards Classic` and `Import Dashboard`

![Import Dashboard](../../../assets/images/03-03-import-dashboard.png)

#### 📌 2 - Modify the Business Health tiles [1 or more]

Modify the business tiles so that they use your specific metrics. 

For `Total Sales Revenue`:
1. Click the tile
1. Click `Configure in Data Explorer`
1. Paste your metric `shop.<NAME>.revenue.actual.count`
1. Click `Run Query`
1. Click `Save Changes to Dashboard`

Repeate these steps for the other tiles (not expected to finsih this task)

#### 📌 3 - Modify the Service Health tiles [1 or more]

Modify the service tiles so that they use your specific metrics. 

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