# About

A testing library that adds support for a BDD style integration test structure. It is designed to allow all integration tests to follow a similar pattern which helps create a consistent test style for a project.

[![https://github.com/CleanKludge/StoryTests](https://img.shields.io/badge/github-StoryTests-brightgreen.svg)](https://github.com/CleanKludge/StoryTests)
[![NuGet Version](http://img.shields.io/nuget/v/StoryTests.svg?style=flat)](https://www.nuget.org/packages/StoryTests/)

# Installation
```
Install-Package StoryTests
```

# Usage

## Basic

You can create a story from a single subject and up to four additional services.

```csharp
string content = await Given.ANewStoryAbout(new HttpClient())
        .With(client => client.DefaultRequestHeaders.Add("X-TESTHEADER", "true"))
    .WhenCalling(client => client.GetAsync("http://cleankludge.com"))
        .Then(response => Assert.That(response.IsSuccessStatusCode))
    .ThenReturn(response => response.Content.ReadAsStringAsync());
```

```csharp
string content = await Given.ANewStoryAbout(new HttpClient(), new HttpRequestMessage(HttpMethod.Get, "http://cleankludge.com"))
        .With(client => client.DefaultRequestHeaders.Add("X-TESTHEADER", "true"))
        .With(message => message.Content = new StringContent("Content"))
    .WhenCalling((client, message) => client.SendAsync(message))
        .Then(response => Assert.That(response.IsSuccessStatusCode))
    .ThenReturn(response => response.Content.ReadAsStringAsync());
```

## Advanced

*Coming soon*

# Methods

*Note: All methods support both sync and async calls*

## Given
```csharp
var story = Given.ANewStoryAbout(Subject);
var story = Given.ANewStoryAbout(Subject, Service1);
var story = Given.ANewStoryAbout(Subject, Service1, Service2);
var story = Given.ANewStoryAbout(Subject, Service1, Service2, Service3);
var story = Given.ANewStoryAbout(Subject, Service1, Service2, Service3, Service4);
```
## With
```csharp
var story = story.With(subject => {});
var story = story.With(service1 => {});
var story = story.With(service2 => {});
var story = story.With(service3 => {});
var story = story.With(service4 => {});
```

## And
```csharp
var story = story.And(subject => {});
var story = story.And(service1 => {});
var story = story.And(service2 => {});
var story = story.And(service3 => {});
var story = story.And(service4 => {});
```

## When
```csharp
var story = story.When(subject => { return result; });
```

## WhenCalling
```csharp
var story = story.When(subject => { return result; });
var story = story.When((subject, service1) => { return result; });
var story = story.When((subject, service1, service2) => { return result; });
var story = story.When((subject, service1, service2, service3) => { return result; });
var story = story.When((subject, service1, service2, service3, service4) => { return result; });
```

## ThenCalling
```csharp
var story = story.ThenCalling((subject, result) => { return newResult; });
var story = story.ThenCalling((service1, result) => { return newResult; });
var story = story.ThenCalling((service2, result) => { return newResult; });
var story = story.ThenCalling((service3, result) => { return newResult; });
var story = story.ThenCalling((service4, result) => { return newResult; });
```

## Then
```csharp
var story = story.Then(result => { });
var story = story.ThenCalling((subject, result) => { });
var story = story.ThenCalling((service1, result) => { });
var story = story.ThenCalling((service2, result) => { });
var story = story.ThenCalling((service3, result) => { });
var story = story.ThenCalling((service4, result) => { });
```

## ThenReturn
```csharp
var newResult = story.ThenReturn(result => { return newResult });
```

## ThenReturnTheResult
```csharp
var result = story.ThenReturnTheResult();
```

# Tips

I like to create my own Given wrappers which allows me to abstract away common setups

```csharp
public class Given
{
    public static IStoryProlog<HttpClient, HttpRequestMessage> ANewGetRequestTo(string uri)
    {
        return new TwoPointProlog<HttpClient, HttpRequestMessage>(new HttpClient(), new HttpRequestMessage(HttpMethod.Get, uri));
    }
}
```

# General Notes

**This is an initial version and not tested thoroughly**.

I've made this library mainly for use in my own projects so use at your own risk :)

# License

StoryTests is released under the [MIT license](https://github.com/Korthax/StoryTests/blob/master/LICENSE.md).
