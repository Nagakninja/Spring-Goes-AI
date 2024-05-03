# Spring-Goes-AI

A spring boot project showcasing interaction with Open-AI using Spring AI Module

## Introduction
It's impossible to spend a day in tech without hearing the words "Artificial Intelligence". In this presentation we will embark on a journey into the world of Artificial Intelligence (AI) specifically designed for beginners. We'll start by introducing the fundamental concepts of AI, demystifying its jargon, and exploring its potential impact on our everyday lives.

Next, we will explore Spring AI. Its goal is to simplify the development of applications that incorporate artificial intelligence functionality, without introducing unnecessary complexity. We will cover the basics of setting up a Spring AI project, how to integrate it with existing Spring Boot applications, and how to use its various components to implement common AI tasks.

Whether you want to add chatbots to your app, generate recommendations, or analyze sentiments in text, Spring AI provides a streamlined and efficient approach to integrating these features. By the end of this talk, you will have a solid grasp of AI basics and how to incorporate them into your Spring applications using Spring AI.

## Agenda

- Getting Started Demo

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

