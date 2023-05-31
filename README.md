# Tago.Extensions.HttpClient

Tago.Extensions.HttpClient is a powerful NuGet package that simplifies the implementation of HTTP REST API methods in .NET projects. It provides extensions to the `HttpClient` class, allowing you to easily add and manage REST API methods. With Tago.Extensions.HttpClient, you can reduce code duplication and improve the maintainability of your codebase.

## Key Features

- **Effortless REST API Method Management**: Tago.Extensions.HttpClient allows you to add REST API methods with different HTTP verbs (GET, POST, PUT, DELETE, etc.) and endpoints, making it easy to organize and maintain your API integration code.
- **Flexible Callbacks**: You can attach custom callback actions to handle the API response for each method, providing the flexibility to process the response according to your application's specific needs.
- **Support for Generic Deserialization**: Tago.Extensions.HttpClient leverages generics to enable seamless deserialization of the API response content into strongly typed objects, eliminating the need for manual parsing.
- **Cancellation Support**: The library supports cancellation using `CancellationToken`, allowing you to cancel ongoing API requests when needed.
- **Global HTTP Call Inspection and Modification**: Tago.Extensions.HttpClient provides the ability to intercept, inspect, modify, or even mock HTTP calls globally. This feature allows you to easily add custom logic or modify requests and responses at a global level, enabling advanced scenarios such as request/response logging, adding authentication headers, or simulating different responses for testing purposes.
- **Integration with Dependency Injection**: Tago.Extensions.HttpClient can be easily integrated into your application's dependency injection container, facilitating the management and configuration of the HTTP client instances.


## Getting Started

To get started with Tago.Extensions.HttpClient, follow these steps:

1. Install the Tago.Extensions.HttpClient NuGet package into your project.
2. Register the RestClient (or Tago.Extensions.HttpClient) as a service in your dependency injection container.
3. Inject the IRestClient instance (or the corresponding interface) into your classes or components that require HTTP client functionality.
4. Use the provided methods to add REST API methods and make HTTP requests.

## Example Usage

Here's an example that demonstrates the basic usage of Tago.Extensions.HttpClient:

#### DI registration:
```csharp
// In your startup configuration or composition root
using Tago.Extensions.Http;

public void ConfigureServices(IServiceCollection services)
{
    // Register TagoHttpClient as a transient service
    services.AddRestClient();
    
    //or
    //services.AddRestClient("clientName");

    // Other service registrations...
}
```

**Using IRestClient**:
```csharp
using Tago.Extensions.Http;

public class MyService
{
    private readonly IRestClient _restClient;

    public MyService(IRestClient restClient)
    {
        _restClient = restClient;
    }

    public async Task DoSomething()
    {
        var response = await _restClient.GetAsync<MyModel>("/api/resource");
        if( response.IsSuccess )
        {
            MyModel model = response.Data;
        
            // Process the response...
        }
    }
}
```

**Or using IRestClientFactory:**

```csharp
public class MyService2
{
    private readonly IRestClient _restClient;

    public MyService2(IRestClientFactory restClientFactory)
    {
        _restClient = restClientFactory.GetClient("clientName");
    }

    public async Task DoSomething(CanncellationToken canncellationToken)
    {
        var response = await _restClient.GetAsync<MyModel>("/api/resource", opts => {
            opts.Headers.Add("Authorization", "token");
        }, canncellationToken);
        
        // Process the response...
    }
}


```

In the above example, the MyService class depends on the IRestClient interface provided by Tago.Extensions.HttpClient. You can then use the _restClient instance to make HTTP requests and handle the API responses in a strongly typed manner.

#### `GetAsync<T>(string endpoint, Action<HttpRequestOptions<T>> options = null, CancellationToken cancellationToken = default)`

Sends an asynchronous GET request to the specified API endpoint and returns an `IApiResponse<T>`.

**Parameters:**

- `endpoint`: The API endpoint to send the GET request to.
- `options` (optional): An action that allows customizing the HTTP request options, such as headers, query parameters, or authentication.
- `cancellationToken` (optional): A cancellation token that can be used to cancel the request.

**Returns:** `Task<IApiResponse<T>>`

- A task representing the asynchronous operation.
- The `IApiResponse<T>` object that encapsulates the HTTP response and deserialized content of type `T`.




#### Gloabl Interception
```csharp
RestClientInterceptor.InterceptRequest = (name, msg) =>
{
    if (!msg.Headers.Contains("Authorization"))
    {
        msg.Headers.TryAddWithoutValidation("Authorization", "Some Token");
    }
};
```

The code snippet demonstrates how to use RestClientInterceptor.InterceptRequest to intercept and modify HTTP requests globally in Tago.Extensions.HttpClient. This feature allows you to add custom logic to inspect and modify requests before they are sent.

In the example code, a callback function is assigned to the RestClientInterceptor.InterceptRequest property. This callback function takes two parameters: **name** and **message**. Inside the callback, it checks if the request message (msg) does not contain an "Authorization" header. If the header is missing, the code adds an "Authorization" header with a default value of "Some Token".

By utilizing this functionality, you can customize and manipulate HTTP requests at a global level, enabling scenarios such as adding authentication headers or modifying request parameters before they are sent. This gives you greater control and flexibility in managing your API integration logic.



## Contributions and Support
Contributions, bug reports, and feature requests are welcome! If you encounter any issues or need assistance, feel free to contact us at support@tago-solutions.com

## License
This project is licensed under the MIT License.






