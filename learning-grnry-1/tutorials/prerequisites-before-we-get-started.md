---
description: There are a few things we need to sort out before we can get going
---

# Prerequisites

## Accessing GRNRY

Yout need to get access credentials to the demo installation of Granary. For that, please sign-in \(or sign-up using your work email\) to our [supportdesk](https://support.grnry.io) and [request access](https://support.grnry.io/support/catalog/items/29).

With the provided credentials you can play through the tutorials.

## Browser

For these tutorials, we also need a browser to interact with the GRNRY UI. Any modern browser will do. We recommend Chrome though.

## Postman

In order to play through this tutorial, we recommend postman in order to interact with the GRNRY RESTful APIs. Almost every functionality is carried out via RESTful services. If you are new to RESTful APIs, W3C schools offers a good [starting point](https://www.w3schools.in/category/restful-web-services/) to dive into the topic.

You can get Postman free [here](https://www.postman.com/downloads/). Although you can use any other tool like `curl` or your browser's developer toolbar for sending HTTP requests to GRNRY, Postman gives you the advantage that you can import a Postman collection.

The [GRNRY Postman collection](../../developer-reference/api-reference/#postman-collection) file contains all the URLs of the GRNRY demo environment plus the implemented methods to interact with GRNRY. With this postman collection, a lot less typing will be needed in this tutorial.

In Postman, you need to provide your [access credentials](prerequisites-before-we-get-started.md) to the GRNRY demo environment:

* Press the eye button \("environment quick look"\) on the upper right corner
* Provide your username and password as current value and press update

## Granary Docs

Before starting the tutorial, make your self familiar with the [data-in structure](../data-in/) as well as the [data transformation and profiling capabilities](../using-data-in-granary/) of Granary.

{% hint style="warning" %}
Check out the [Granary glossary](../granary-glossary.md) whenever you encounter an unknown term.
{% endhint %}

