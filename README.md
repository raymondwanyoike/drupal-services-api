# Drupal Services API Client (Java)

This library is an easy-to-use Java client library for accessing the Drupal [Services](http://drupal.org/project/services) REST Server API.

## Introduction

This library makes use of [Retrofit](https://github.com/square/retrofit) as a REST client. For more information visit the [Retrofit website](https://square.github.io/retrofit).

----

The RestAdapter class generates an implementation of a resource interface.

```java
RestAdapter restAdapter = new RestAdapter.Builder()
        .setEndpoint("http://127.0.0.1/endpoint") // Service endpoint
        .build();

NodeResourceForm resource = restAdapter.create(NodeResourceForm.class);
```

Each call on a generated resource makes an HTTP request to the webserver.

```java
Map<String, String> params = new LinkedHashMap<String, String>();

params.put("pagesize", "10"); // Number of items to be returned
params.put("page", "2"); // Page number of results to return
params.put("fields", "nid,title"); // Fields you want returned
params.put("parameters[\"type\"]", "article"); // Values used to filter results
params.put("parameters[\"status\"]", "1"); // ...

List<NodeEntity> nodes = resource.index("node", params);
```

## Sessions

The SessionInterceptor class inserts a session header, and CSRF token.

```java
SessionInterceptor interceptor = new SessionInterceptor();

RestAdapter restAdapter = new RestAdapter.Builder()
        .setEndpoint("http://127.0.0.1/endpoint") // Service endpoint
        .setRequestInterceptor(interceptor)
        .build();

UserResourceForm resource = restAdapter.create(UserResourceForm.class);

UserEntity user = new UserEntity()
        .setName("random") // Username
        .setPass("hackme"); // Password

Map<String, String> params = new EntityConverter<UserEntity>().convert(user);

SessionEntity session = resource.login("user", params);

interceptor.setSession(session); // Requests now carry a session/token
```

When done.

```java
resource.logout("user"); // Logout of Drupal (invalidate session)

interceptor.clearSession(); // Requests are now anonymous
```

## *ResourceForm, and *ResourceJson

TODO: Explain Retrofit, GSON, and Drupal Services REST Server API limits.
