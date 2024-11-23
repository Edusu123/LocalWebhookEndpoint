This code was created following the instructions of a [tutorial](https://www.youtube.com/watch?v=41N5oni1ffQ "Les Jackson - Get webhooks at localhost") and the goal was to configure an API to receive request from the Github Webhooks integration.

Important Info:
* .NET 9.0.100
* In the lines below remains my best attempt to summarize the basic steps to understand how the integration works.

## Tutorial

1. The first step is to create your api to process the requests from Github and do whatever you want. And you can do so by creating a minimal API with .NET!

Using the .NET CLI ([doc](https://learn.microsoft.com/pt-br/dotnet/core/tools/)) you can run `dotnet new webapi -n LocalWebhookEndpoint` (at least that's what I did) and see the project being created.

2. Then you can implement this endpoint so you are able to check the incoming requests: 
```cs
app.MapPost("/webhooks", (HttpContext context) => {
    Console.ForegroundColor = ConsoleColor.Green;
    Console.WriteLine("We've got a hit!");

    var headers = context.Request.Headers;

    foreach(var header in headers){
        Console.WriteLine($"{header.Key}:{header.Value}");
    }

    Console.ResetColor();

    return Results.Ok();
});
```

3. After that you should install ngrok (i used chocolatey to install on windows), and you can to that in [here](https://dashboard.ngrok.com/get-started/setup/windows "ngrok configuration process").

4. Having configured ngrok on your machine, you can run the api on a localhost port and then run `ngrok http https://localhost:port`. The following information should appear on your screen:

<p align="center">
  <img alt="test" src="https://github.com/user-attachments/assets/42315c6c-d92a-4984-8289-2e2afbdadaac" width="1024">
</p>

5. And for the last part, we have to configure the github integration like the following:

<p align="center">
  <img alt="test" src="https://github.com/user-attachments/assets/eae5b26a-96d2-424c-990f-634e1c65ed4b" width="1024">
</p>

6. After selecting the integration, you need to inform the ngrok url and select "send me _everything_" (that will trigger the webhook whenever any action is performed on the repo - including creating issues).

<p align="center">
  <img alt="test" src="https://github.com/user-attachments/assets/1cf750a1-8b00-4ea1-b850-adf54a57c8bb" width="512">
</p>

7. Having created all this you can test it by creating a new issue on the repo:

<p align="center">
  <img alt="test" src="https://github.com/user-attachments/assets/e9380d4b-3d26-47dc-8cf1-618ed0bbefff" width="1000">
</p>

8. And here are the details of the request we received!

<p align="center">
  <img alt="test" src="https://github.com/user-attachments/assets/1c8a31f0-18d4-4ead-a63d-8308b3725058" width="700">
</p>

And that's it!

Now, bear in mind that an integration in production can't rely on the dynamic links of free version ngrok. So you will have to pay something for that.

And we should all pay attention to the possibilities of this event handling. Creating a developer tool that integrates those events? Or improving the visibility of an already existing software? Who knows?! The possibiilties are various... 

<p align="center">
  <img alt="test" src="https://github.com/user-attachments/assets/02a435cb-9296-4ed3-a1e0-c908478fcafd" width="512">
</p>
