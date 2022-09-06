---
title: GOLANG DOCUMENTATION
date: 2022-09-01
publishdate: 2022-09-02
weight: 20
menu:
  main:
    weight: 20
---

To integrate golang web services with API Toolkit, an SDK called the golang client for API Toolkit is utilized. It keeps track of incoming traffic, aggregates the requests, and then delivers them to the apitoolkit servers.We'll concentrate on providing a step-by-step instruction for integrating an API toolkit into our Golang web service in this tutorial. 

## Design decisions:
- The SDK relies on google cloud pubsub over grpc behind the scenes, to ensure that your traffic is communicated to APIToolkit for processing in the most efficient ways.
- Processing the live traffic in this way, allows :
  1. APIToolkit to perform all kinds of analysis and anomaly detection and monitoring on your APIs in real time.
  2. Users to explore their API live, via the api log explorer.

## How to Integrate with Gin router:
1. Fisrt thing first - Aloways check if your codes are running properly before going ahead to integrate
2. Sign up to the API dashboard
3. Generate your API key by creating a project and include a little description of your work. And don't forget to copy your key when its been generated to avoid loosing it.
4. Initialize the middleware with the APItoolkit API key like you see below:

``` go
package main

import (
  	// Import the apitoolkit golang sdk
  	apitoolkit "github.com/apitoolkit/apitoolkit-go"
)

func main() {
  	// Initialize the client using your apitoolkit.io generated apikey
  	apitoolkitClient, err := apitoolkit.NewClient(
      context.Background(), apitoolkit.Config{APIKey: "<APIKEY>"},
    )
    ...

  	router := gin.New()

  	// Register with the corresponding middleware of your choice. For Gin router, we use the GinMiddleware method.
  	router.Use(apitoolkitClient.GinMiddleware)

  	// Register your handlers as usual and run the gin server as usual.
  	router.POST("/:slug/test", func(c *gin.Context) {c.Text(200, "ok")})
 	...
}
```
3. Deploy your application and enjoy exploring and managing your API via the APIToolkit dashboards and API log explorer
