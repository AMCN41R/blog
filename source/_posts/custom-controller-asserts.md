---
title: 'Unit Testing C# Controllers'
tags:
  - Unit Testing
  - XUnit
  - 'C# testing'
  - .NET testing
  - dotnet testing
  - controller testing
  - contract testing
photos: /images/custom-controller-asserts/header.jpg
date: 2020-06-06 13:18:33
---


I'm a huge proponent of automated testing in all forms. There is always a balance to find between the effort required to write a test case vs the benefit *of* that test case, but with some help, you can reduce that effort.

## Contract Testing
First, I'd like to (briefly) talk about contract testing.

Contract testing is defined nicely [in this blog](https://pactflow.io/blog/what-is-contract-testing/) by the guys at [Pact Flow](https://pactflow.io/).

> *"Contract testing is a methodology for ensuring that two separate systems (such as two microservices) are compatible with one other. It captures the interactions that are exchanged between each service, storing them in a contract, which can then be used to verify that both parties adhere to it."*

In my humble opinion, this is great strategy to employ when working on any large scale application or solution. Contract testing can give us confidence that our APIs do not break any upstream integrations or clients. As a result, we can move faster and [deliver value more quickly](https://amcn41r.github.io/blog/2017/05/19/just-the-salt/) and safely.

## A quick win?
If true contract testing seems a bit far off right now, an interim step might be to write unit tests against your controller methods. With the help of reflection, we can test things like the route and method attributes as well as the implementation.

For example, this static method can be used to check that a controller method has the expected `HttpMethodAttribute` with the correct route template:
```csharp
public static void MethodHasVerb<TController, TVerbAttribute>(string methodName, string template)
    where TController : ControllerBase
    where TVerbAttribute : HttpMethodAttribute
{
    var method = typeof(TController).GetMethod(methodName);

    var attr = method?.GetCustomAttributes(typeof(TVerbAttribute), false).ToList();

    if (attr == null)
    {
        throw new Exception("No attributes found.");
    }

    // this methods checks that the required attribute is present only once
    attr.AssertAttributeCount<TVerbAttribute>();

    var verb = (HttpMethodAttribute) attr[0];

    Assert.Equal(template, verb.Template);
}
```

Assuming that the method above is a member of the class `AssertController`, it can be used like this...
```csharp
// method under test in OrderController
[HttpGet("{orderId}/products")]
public async Task<IActionResult> GetProductsForOrder(Guid orderId)
{
    // implementation
}

// unit test (using XUnit)
[Fact]
public void GetProductsForOrder_Method_HasCorrectVerbAttributeAndPath()
{
    AssertController
        .MethodHasVerb<OrderController, HttpGetAttribute>(
            nameof(OrderController.GetProductsForOrder),
            "{orderId}/products"
        );
}
```

#### How easy is that?!
With very little effort, we have an automated test that ensures that our original contract of Verb and Route does not change.


## Going a little further...
With a couple of extra helper methods, you can apply the same approach for methods without a route template, and to the controller itself...

```csharp
public static void MethodHasVerb<TController, TVerbAttribute>(string methodName)
    where TController : ControllerBase
    where TVerbAttribute : HttpMethodAttribute
{
    var method = typeof(TController).GetMethod(methodName);

    var attr = method?.GetCustomAttributes(typeof(TVerbAttribute), false).ToList();

    attr.AssertAttributeCount<TVerbAttribute>();
}

public static void HasRouteAttribute<TController>(string route)
    where TController : ControllerBase
{
    var attr = typeof(TController).GetCustomAttributes(typeof(RouteAttribute), false).ToList();

    attr.AssertAttributeCount<RouteAttribute>();

    Xunit.Assert.Equal(route.ToLower(), ((RouteAttribute)attr[0]).Template.ToLower());
}
```

I've created a [gist on GitHub](http://gist.github.com/AMCN41R/7331c282a60c46f1b9ee5a0fc0933d9a) with these methods and some examples.


## Tests Passed
Some might think this approach a little overboard, but I think for relatively little effort, you can get some peace of mind that the api you designed is still in place as your application grows.

Of course, this does not guarantee the implementation doesn't change, but that's a topic for another day :smiley:
