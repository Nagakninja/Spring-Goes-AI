# Spring-Goes-AI

A spring boot project showcasing interaction with Open-AI using Spring AI Module

## Introduction
It's impossible to spend a day in tech without hearing the words "Artificial Intelligence". In this presentation we will embark on a journey into the world of Artificial Intelligence (AI) specifically designed for beginners. We'll start by introducing the fundamental concepts of AI, demystifying its jargon, and exploring its potential impact on our everyday lives.

Next, we will explore Spring AI. Its goal is to simplify the development of applications that incorporate artificial intelligence functionality, without introducing unnecessary complexity. We will cover the basics of setting up a Spring AI project, how to integrate it with existing Spring Boot applications, and how to use its various components to implement common AI tasks.

Whether you want to add chatbots to your app, generate recommendations, or analyze sentiments in text, Spring AI provides a streamlined and efficient approach to integrating these features. By the end of this talk, you will have a solid grasp of AI basics and how to incorporate them into your Spring applications using Spring AI.

## Agenda

- Getting Started Demo
- Prompts Demo
  - SimplePrompt
  - DadJokesController
  - YouTube

## Getting Started Demo

In this demo you will create a simple `ChatController` that can send a message to OpenAI.

```java
@RestController
public class ChatController {

  private final ChatClient chatClient;

  public ChatController(ChatClient chatClient) {
    this.chatClient = chatClient;
  }

  @GetMapping("/api/generate")
  public Map generate(@RequestParam(value = "message", defaultValue = "Tell me a Dad Joke") String message) {
    return Map.of("generation",chatClient.call(message));
  }

}
```

Request

```
http :8080/api/generate
```

Response

```
HTTP/1.1 200
Connection: keep-alive
Content-Type: application/json
Date: Wed, 27 Mar 2024 20:43:20 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked

{
    "generation": "Why don't scientists trust atoms?\n\nBecause they make up everything!"
}
```
## Prompts Demo


### SimplePrompt

Walk through the Prompt class and its different constructors.

```java
@RestController
public class SimplePrompt {

  private final ChatClient chatClient;

  public SimplePrompt(ChatClient chatClient) {
    this.chatClient = chatClient;
  }

  @GetMapping("/api/simple-prompt")
  public String simple() {
    return chatClient.call(
                             new Prompt("How long has The Java Programming language been around?"))
                     .getResult().getOutput().getContent();
  }
}
```

### DadJokesController

Show the different types of messages

```java
@RestController
public class DadJokeController {

  private final ChatClient chatClient;

  public DadJokeController(ChatClient chatClient) {
    this.chatClient = chatClient;
  }

  @GetMapping("/api/jokes")
  public String jokes() {
    var system = new SystemMessage("You primary function is to tell Dad Jokes. If someone asks you for any other type of joke please tell them you only know Dad Jokes");
    var user = new UserMessage("Tell me a joke");
//        var user = new UserMessage("Tell me a very serious joke about the earth");
    Prompt prompt = new Prompt(List.of(system, user));
    return chatClient.call(prompt).getResult().getOutput().getContent();
  }
}
```

###  YouTube

Show off `PromptTemplate` by using a simple string

```java
@GetMapping("/popular-step-one")
public String findPopularYouTubersStepOne(@RequestParam(value = "genre", defaultValue = "tech") String genre) {
        String message = """
        List 10 of the most popular YouTubers in {genre} along with their current subscriber counts. If you don't know
        the answer , just say "I don't know".
        """;
        PromptTemplate promptTemplate = new PromptTemplate(message);
        Prompt prompt = promptTemplate.create(Map.of("genre",genre));
        return chatClient.call(prompt).getResult().getOutput().getContent();
        }
```

And then externalizing that to a classpath resource

```java
@GetMapping("/popular")
public String findPopularYouTubers(@RequestParam(value = "genre", defaultValue = "tech") String genre) {
        PromptTemplate promptTemplate = new PromptTemplate(ytPromptResource);
        Prompt prompt = promptTemplate.create(Map.of("genre", genre));
        return chatClient.call(prompt).getResult().getOutput().getContent();
        }
```
