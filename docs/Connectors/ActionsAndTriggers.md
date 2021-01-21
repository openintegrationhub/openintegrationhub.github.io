---
layout: default
title: Actions and Triggers
nav_order: 2
parent: Connectors
has_children: true
---
# Actions and Triggers
As laid out in the development example, actions and triggers are the part of any component, that really defines what you can do with it. Most parts like credentials or the component.json are fairly standard and reusable. Actions and triggers however have to be specifically adapted to your solutions API.

This guide helps to classify your API and derive a set of functionalities that can be performed by an Adapter. We focus on the Adapter, as the Transformer is even simpler. So once you understand the Adapter, the rest is a piece of cake.

The Adapter is a single, reusable piece of functionality that stands between your solution’s API and the Transformer. To enable communication between you and Open Integration Hub, the Adapter syntactically normalizes and transforms your applications data into a JSON object. For example, transforming CSV-, JS-, XML- files into JSON objects. The Adapter exposes the endpoint of your SaaS solution’s API via pre-defined actions and triggers. Those make sure that the four basic operations of persistent storage are available, such as create, read, update and delete a file.

## Table of Contents

  - [Given an API how should an Adapter behave](#given-an-api-how-should-an-adapter-behave)
      - [Questions](#questions)
          - [Question 1: Is the list of business objects dynamic](#question-1-is-the-list-of-business-objects-dynamic)
          - [Question 2: Is the structure of objects dynamic](#question-2-is-the-structure-of-objects-dynamic)
          - [Question 3: Does the API support Webhooks](#question-3-does-the-api-support-webhooks)
  - [Desired Adapter Behavior](#desired-adapter-behavior)


## Given an API how should an Adapter behave

The expected actions and triggers of an adapter depend on the behavior of the
API.  If the API supports CRUD operations the following diagram explains which
triggers and actions should exist in the adapter.  The triggers and actions
should aim at covering 100% of the objects provided by the API.

 **Note:** Although RESTful APIs are preferred, the API does not necessarily have
 to be a REST API.  It is possible for the above functionality to be exposed via
 a SOAP API, a SQL (or other) DB connection, etc.

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="870px" height="669px" viewBox="-0.5 -0.5 870 669" content="&lt;mxfile modified=&quot;2018-12-04T11:01:47.585Z&quot; host=&quot;www.draw.io&quot; agent=&quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36&quot; version=&quot;9.5.5&quot; etag=&quot;HrSlzaE6Hqm4dZ9jRYBa&quot; type=&quot;device&quot;&gt;&lt;diagram name=&quot;Page-1&quot; id=&quot;8ce9d11a-91a2-4d17-14d8-a56ed91bf033&quot;&gt;7Vxdc5s4FP01fuwOQgjwY/PV7czutrPpbJNHBcuGBiMPyInTX7+SkWwj4YSlCBwveUgsIWT53nPvObrImcDL5eZTjlfxn3RG0onrzDYTeDVxXeD7Pv8jel7KngAEZcciT2Zy0L7jNvlJZKcje9fJjBSVgYzSlCWramdEs4xErNKH85w+V4fNaVp91xVeEKPjNsKp2fs9mbFY9gLH2V/4nSSLWL51iOSFBxw9LnK6zuT7TVw43/6Ul5dYzSXHFzGe0eeDLng9gZc5pax8tdxcklTYVpmtvO/myNXdunOSsUY3BAT7fkAAily+1OiDO5ULYy/KGGTGbSObNGcxXdAMp9f73ovtByZiSsBbMVum8mWKH0h6sbPJJU1pzi9lNBO3FQzn7KNwl9Z3k6RiBke1JUAQb5Nspu6IUlwUSfQtTrLygrwNlK2Dm34Qxl5kG68Z5V37D/IHpSt5V8Fy+kjUKrnvnO3P7orCghg7pxm7wcskFRD/h+QznGHZLd8plM26+Uw/SdcVdJ1H5JhzZCjgfEHYkTGwHCOcdjCx9P4nQpeE5S98QE5SzJKnKuCxjJvFbtweO/yFhE9TKJVzPuF0Ld/lcyFWHxP+O00Kxv/QuQiadZFkpBAX6cMPHs/FNnVk3LrRBN4YgKzC7TlOGLld4a3Vnnk+qkJwF2FOAzz+B/dzrB2MlEHeBBbAfQUXOE0WmcA2xwXJXwPKE8kZ2bzqYnkVqawlk7EK8eeDzKbyUXyY1BwLoICO4c4xv3SfXzha8pc78Rl/Q6p5Lz9yu9wDG+SesOvcI2/9SpOM7QHthxqggYbUcpHyLg2su2W0xC8Y8Xu2+A36wa+HBsSvQcp3X/42IZ2mXGqTt9m1iuyRa5FXdS1Q7QOy9Wq41rPBtYHh6ytK9hLs49fPwuvr1YoHu1gieYgpfSxGyfXLMOBbxQoMPAD701xmiP8SZTlV5zbliu4zeGhm8M4TdlMb+ydvY7JJ2J0ax1/f74Y1sn5gGtvrhx1DV2NHZdse2DE03HqwZeUZaR2xdU7UvnXcrHafOX3viDbqI3OaFYuRMIepUXjA7c/t3uj2gdweDul2lVkGJvG2dOz2Q8doWnUR9K3RsblfGfemXVKrVjaDnhls1vamprK6xIXIrqi9g3GxKh/+zZON8HMXRgo09ekD3zBSaCshTY8ZyT8tI00HNFINWw+RtS1uvVxkJnsFDdvZXvcsUjlCTVEu1Mj2xkRBoEHEsUYbyl4jb1gSadqWDEHUG2+8o0KL02G09/QgLdAeRCDQMtpDqEPEXrSbKnEQRCivl4CQfu+k8OmZgFAgsV5708Q+CtoCwtd5JLAFCGjWu6Vqgu05wIZq8gZUTdBUTdJI3mkZyYcDGskUEqdKNi2lJfTN3AJ7quvrhYTW0tLzepOW0JQfo7S0+Li8T2kJT0RI2JOWtdHe+fnQI0GqbyTbSkv9yYBFaQmPFqlqDn0NSZJAnULbkWTQH0keLVLVhM+QRoIDGknlp3eQW1oqCU99LeMwt/RUpIJu1bPIg+1yCwg0iDjaRN3lFmWvUUlYOnGFNExAc99gS0koIfsOor2lkqiN9p6KVED3LGgZ7S7UIWIv2k//DN7xAlYjRBw7bFo5MAb6QQin9qpjg7Z8MNUmmtpDyPt5jNUSIW5NyvD7AYRRatDPLDQFhH4aDU2tbT6QKRnvr28NTHA6ZJoSIEXyEz9sBwi3rcRitstDFxN0JfiWA6CQvHxAvymZi6kExyYRTj/KbibAcVFw5ZFki29bpHzwOiJpoGdg86todSTtdkDSyBRgf305L/NC523zQlvmNTXQ2cHXLEWE/cHXVBTnBl8ja9eY1xp8TTo+O/jqhy17ha9ZbD03+BrPTvuEr1m5PDv46tkX9CkezKLnucFXz7515rUFX/9/oH31rQRs+M3QTux7/to3gG+btyP48ub+H/SUO739f0GC1/8C&lt;/diagram&gt;&lt;/mxfile&gt;" style="background-color: rgb(255, 255, 255);"><defs/><path d="M 429 80 L 429 90 Q 429 100 429 95.5 L 429 93.25 Q 429 91 429 98.19 L 429 105.38" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 429 109.88 L 427 103.88 L 429 105.38 L 431 103.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><rect x="369" y="0" width="120" height="80" rx="12" ry="12" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(370.5,19.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="116" height="40" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 116px; white-space: normal; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Is the list of business objects dynamic?</div></div></foreignObject><text x="58" y="26" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">Is the list of business objects dynamic?</text></switch></g><path d="M 449 131 L 559 131 Q 569 131 569 141 L 569 175.38" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 569 179.88 L 567 173.88 L 569 175.38 L 571 173.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 409 131 L 150 131 Q 140 131 140 141 L 140 378.38" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 140 382.88 L 138 376.88 L 140 378.38 L 142 376.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="429" cy="131" rx="20" ry="20" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(415.5,124.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="26" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 26px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">XOR</div></div></foreignObject><text x="13" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">XOR</text></switch></g><rect x="80" y="384" width="120" height="80" rx="12" ry="12" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(81.5,403.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="116" height="40" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 116px; white-space: normal; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Does the API support webhooks?</div></div></foreignObject><text x="58" y="26" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">Does the API support webhooks?</text></switch></g><path d="M 569 261 L 569 306.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 569 311.88 L 565.5 304.88 L 569 306.63 L 572.5 304.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 589 333 L 729 333 L 729 376.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 729 381.88 L 725.5 374.88 L 729 376.63 L 732.5 374.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><rect x="509" y="181" width="120" height="80" rx="12" ry="12" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(510.5,207.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="116" height="26" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 116px; white-space: normal; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Is the structure of objects dynamic?</div></div></foreignObject><text x="58" y="19" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">Is the structure of objects dynamic?</text></switch></g><rect x="369" y="383" width="120" height="80" rx="12" ry="12" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(370.5,402.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="116" height="40" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 116px; white-space: normal; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Does the API support webhooks?</div></div></foreignObject><text x="58" y="26" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">Does the API support webhooks?</text></switch></g><rect x="669" y="383" width="120" height="80" rx="12" ry="12" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(670.5,402.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="116" height="40" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 116px; white-space: normal; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Does the API support webhooks?</div></div></foreignObject><text x="58" y="26" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">Does the API support webhooks?</text></switch></g><path d="M 549 333 L 429 333 L 429 376.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 429 381.88 L 425.5 374.88 L 429 376.63 L 432.5 374.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="569" cy="333" rx="20" ry="20" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(555.5,326.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="26" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 26px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">XOR</div></div></foreignObject><text x="13" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">XOR</text></switch></g><ellipse cx="629" cy="627" rx="40" ry="40" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(609.5,620.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="38" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 40px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Case 5</div></div></foreignObject><text x="19" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">Case 5</text></switch></g><ellipse cx="829" cy="627" rx="40" ry="40" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(809.5,620.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="38" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 40px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Case 6</div></div></foreignObject><text x="19" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">Case 6</text></switch></g><path d="M 749 526 L 829 526 L 829 580.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 829 585.88 L 825.5 578.88 L 829 580.63 L 832.5 578.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="729" cy="526" rx="20" ry="20" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(715.5,519.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="26" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 26px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">XOR</div></div></foreignObject><text x="13" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">XOR</text></switch></g><path d="M 709 526 L 629 526 L 629 580.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 629 585.88 L 625.5 578.88 L 629 580.63 L 632.5 578.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 729 463 L 729 499.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 729 504.88 L 725.5 497.88 L 729 499.63 L 732.5 497.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="329" cy="627" rx="40" ry="40" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(309.5,620.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="38" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 40px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Case 3</div></div></foreignObject><text x="19" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">Case 3</text></switch></g><ellipse cx="539" cy="627" rx="40" ry="40" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(519.5,620.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="38" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 40px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Case 4</div></div></foreignObject><text x="19" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">Case 4</text></switch></g><path d="M 449 526 L 539 526 L 539 580.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 539 585.88 L 535.5 578.88 L 539 580.63 L 542.5 578.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="429" cy="526" rx="20" ry="20" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(415.5,519.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="26" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 26px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">XOR</div></div></foreignObject><text x="13" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">XOR</text></switch></g><path d="M 409 526 L 329 526 L 329 580.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 329 585.88 L 325.5 578.88 L 329 580.63 L 332.5 578.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="40" cy="628" rx="40" ry="40" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(20.5,621.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="38" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 40px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Case 1</div></div></foreignObject><text x="19" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">Case 1</text></switch></g><ellipse cx="240" cy="628" rx="40" ry="40" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(220.5,621.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="38" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 40px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">Case 2</div></div></foreignObject><text x="19" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">Case 2</text></switch></g><path d="M 160 527 L 240 527 L 240 581.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 240 586.88 L 236.5 579.88 L 240 581.63 L 243.5 579.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><ellipse cx="140" cy="527" rx="20" ry="20" fill="#ffffff" stroke="#000000" pointer-events="none"/><g transform="translate(126.5,520.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="26" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Verdana; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; width: 26px; white-space: nowrap; overflow-wrap: normal; text-align: center;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">XOR</div></div></foreignObject><text x="13" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Verdana">XOR</text></switch></g><path d="M 120 527 L 40 527 L 40 581.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 40 586.88 L 36.5 579.88 L 40 581.63 L 43.5 579.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 140 464 L 140 500.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 140 505.88 L 136.5 498.88 L 140 500.63 L 143.5 498.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 429 463 L 429 499.63" fill="none" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><path d="M 429 504.88 L 425.5 497.88 L 429 499.63 L 432.5 497.88 Z" fill="#000000" stroke="#000000" stroke-miterlimit="10" pointer-events="none"/><g transform="translate(81.5,512.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="24" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">YES</div></div></foreignObject><text x="12" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">YES</text></switch></g><g transform="translate(171.5,512.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="18" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">NO</div></div></foreignObject><text x="9" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">NO</text></switch></g><g transform="translate(370.5,511.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="24" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">YES</div></div></foreignObject><text x="12" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">YES</text></switch></g><g transform="translate(460.5,511.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="18" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">NO</div></div></foreignObject><text x="9" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">NO</text></switch></g><g transform="translate(670.5,511.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="24" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">YES</div></div></foreignObject><text x="12" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">YES</text></switch></g><g transform="translate(760.5,511.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="18" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">NO</div></div></foreignObject><text x="9" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">NO</text></switch></g><g transform="translate(370.5,112.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="24" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">YES</div></div></foreignObject><text x="12" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">YES</text></switch></g><g transform="translate(460.5,112.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="18" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">NO</div></div></foreignObject><text x="9" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">NO</text></switch></g><g transform="translate(510.5,313.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="24" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">YES</div></div></foreignObject><text x="12" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">YES</text></switch></g><g transform="translate(600.5,313.5)"><switch><foreignObject style="overflow:visible;" pointer-events="all" width="18" height="12" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; font-size: 12px; font-family: Helvetica; color: rgb(0, 0, 0); line-height: 1.2; vertical-align: top; white-space: nowrap;"><div xmlns="http://www.w3.org/1999/xhtml" style="display:inline-block;text-align:inherit;text-decoration:inherit;">NO</div></div></foreignObject><text x="9" y="12" fill="#000000" text-anchor="middle" font-size="12px" font-family="Helvetica">NO</text></switch></g></svg>

### Questions

#### Question 1: Is the list of business objects dynamic

Some systems have a fixed list of objects (and corresponding API endpoints)
which exist in the system.  Other systems allow users and admins to create new
types of objects (which result in the system dynamically creating API
endpoints).  In this case, it is common for the system to provide an API
endpoint which can provide a list of all dynamic objects as well as the
structure of every object.

If the system has a fixed list of objects, then the adapter developer can
provide a hard-coded list of objects that the adapter can interact with.  

If the list of objects is dynamic and it is possible to use the API to learn the
list of existing objects, the developer should write code to fetch that list and provide that as dynamic
configuration.

Furthermore, in the case of a dynamic list of business objects, since the list
of objects is unknown, the structure of objects is also unknown.  This means
that an answer of *yes* to *Question 1* implies an answer of *yes* to *Question
2*.

#### Question 2: Is the structure of objects dynamic

*As explained above, an answer of yes to Question 1 implies an answer of yes to
Question 2.*

Some systems have a fixed list of fields on every object.  Other systems allow
users and admins to customize the structure of each object.  In this case, it is
common for the system to provide an API endpoint which can provide the structure
of any object in the system.

If the objects have a fixed structure, then the adapter developer can hard code
the schema of the objects produced.  Otherwise, the developer should write code
to fetch the dynamic structure of that object.

#### Question 3: Does the API support Webhooks

Some external systems support the concept of **Webhooks**.  The idea of a hook
is that when a change occurs to an object in that external system, that external
system proactively informs other systems about this change.

Webhooks are hooks where the information is transferred by having the system reporting the change make a REST API call to the system making the change.  [See here for more
information about the
webhook service.](https://github.com/openintegrationhub/openintegrationhub/tree/c605c602269f9454af5e2027b2e0d4a4b84b48ef/services/webhooks)

This can be more efficient (both in terms of speed and machine resources) than
having a scheduled job periodically make calls for changes that may or may not
have occurred.

## Desired Adapter Behavior

### Case 1

- The list of business objects is dynamic
- The structure of the objects is dynamic (*Implied by above statement*)
- The API supports webhooks

#### Triggers

- [getObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling)
 including functionality to
    - supply the list of readable objects
- [getObjectsWebhook](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-objects-webhook)
 including functionality to
    - supply the list of readable objects
    - supply the structure of the incoming objects
- [getDeletedObjectsWebhook](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-deleted-objects-webhook)
 including functionality to
    - supply the list of deletable objects

#### Actions

- [upsertObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#upsert-object)
 including functionality to
    - supply the list of writable objects
    - supply the structure of the incoming object
- [deleteObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#delete-object)
including functionality to
    - supply the list of deletable objects
- [lookupObjectByField](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#lookup-object-at-most-1)
 including functionality to
    - supply the list of readable objects
    - supply the list of fields that can be searched

### Case 2

- The list of business objects is dynamic 
- The structure of the objects is dynamic (*Implied by above statement*)
- The API does not support webhooks 

#### Triggers

- [getObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling)
 including functionality to
   - supply the list of readable objects
- [getDeletedObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling) (if possible)
  - including functionality to supply the list of deletable objects

#### Actions

- [upsertObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#upsert-object)
 including functionality to
    - supply the list of writable objects
    - supply the structure of the incoming object
- [deleteObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#delete-object)
 including functionality to
   - supply the list of deletable objects
- [lookupObjectByField](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#lookup-object-at-most-1)
 including functionality to
   - supply the list of readable objects
   - supply the list of fields that can be searched

### Case 3

- The list of business objects is static
- The structure of the objects is dynamic
- The API supports webhooks

#### Triggers

- [getObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling) including
  - the static list of readable objects
- [getObjectsWebhook](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-objects-webhook) including
  - the static list of readable objects
  - functionality to supply the structure of the incoming objects
- [getDeletedObjectsWebhook](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-deleted-objects-webhook) including
  - the static list of deletable objects

#### Actions

- [upsertObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#upsert-object) including
    - the static list of writable objects
    - functionality to supply the structure of the incoming object
- [deleteObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#delete-object) including
    - the static list of deletable objects
- [lookupObjectByField](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#lookup-object-at-most-1) including functionality to
    - the static list of readable objects
    - supply the list of fields that can be searched

### Case 4

- The list of business objects is static
- The structure of the objects is dynamic
- The API does not support webhooks

#### Triggers

- [getObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling) including
    - the static list of readable objects
- [getDeletedObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling) (if possible)
    - including the static list of readable objects

#### Actions

- [upsertObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#upsert-object) including
    - the static list of writable objects
    - functionality to supply the structure of the incoming object
- [deleteObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#delete-object) including
    - the static list of deletable objects
- [lookupObjectByField](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#lookup-object-at-most-1) including functionality to
    - the static list of readable objects
    - supply the list of fields that can be searched

### Case 5

- The list of business objects is static
- The structure of the objects is static
- The API supports webhooks

#### Triggers

- [getObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling) including
    - the static list of readable objects
- [getObjectsWebhook](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-objects-webhook) including
    - the static list of readable objects
    - the static structure of the incoming objects
- [getDeletedObjectsWebhook](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-deleted-objects-webhook) including
    - the static list of deletable objects

#### Actions

- [upsertObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#upsert-object) including
    - the static list of writable objects
    - the static structure of the incoming object
- [deleteObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#delete-object) including
    - the static list of deletable objects
- [lookupObjectByField](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#lookup-object-at-most-1) including functionality to
    - the static list of readable objects
    - the static list of fields that can be searched

### Case 6

- The list of business objects is static
- The structure of the objects is static
- The API does not support webhooks

#### Triggers

- [getObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-new-and-updated-objects-polling) including
    - the static list of readable objects
- [getDeletedObjectsPolling](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#get-deleted-objects-polling) (if possible)
    - including the static list of readable objects

#### Actions

- [upsertObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#upsert-object) including
    - the static list of writable objects
    - the static structure of the incoming object
- [deleteObject](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#delete-object) including
    - the static list of deletable objects
- [lookupObjectByField](https://github.com/openintegrationhub/Connectors/blob/master/Adapters/AdapterBehaviorStandardization/StandardizedActionsAndTriggers.md#lookup-object-at-most-1) including functionality to
    - the static list of readable objects
    - the static list of fields that can be searched
