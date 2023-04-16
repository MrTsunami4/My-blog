+++
title="Basic REST API with Axum"
description="Short introduction on how to develop an api with axum"
date = 2022-05-13

[taxonomies]
tags = ["REST", "rust", "computer", "web"]

[extra]
ToC = true
+++

# API with Axum

## What is Axum ?

[Axum](https://github.com/tokio-rs/axum) is a relatively new Rust web framework, build on top of solid foundation :

- [Tokio](https://tokio.rs/), the most popular Rust async runtime, for very high performance highly parallel execution

- [Hyper](https://hyper.rs/), a correct implementation of the HTTP protocol, and you know, if you have read [this article](https://fasterthanli.me/articles/i-want-off-mr-golangs-wild-ride) can be harder than you might think

- and [Tower](https://github.com/tower-rs/tower), an already rich library of components, that you will use most of the time as middleware.

This project, who the first release is just one year old, as for author [David Pedersen](https://github.com/davidpdrsn), who is an engineer at Embark studio, but most importantly, a member of the tokio team, which for a lot of people made Axum the official tokio web framework.

## The other tools

To build an API, you still need some important tool, since Axum is not a battery-included framework, like a lot of other rust crate. The other package you will need are :

- [sqlx](https://github.com/launchbadge/sqlx), to make queries to the database, with async support,

- [jsonwebtoken](https://github.com/Keats/jsonwebtoken), if you need authentication

- and a bunch of other utils crate, like anyhow or once_cell

## Now, to the fun part

### The router

The router is the best part to begin the design of your API with. Because it's what the internet will see once your project is online, it define the capability and features of your website, so try to not change it to much.

You first define path, that are the URLs that you want to handle. You then add function that handle common HTTP verbs, like `get` and `post`. You can also add extensions, that are the states that you want to share across the API.

```rust
Router::new()
        .route("/", get(read_all_todos).post(create_todo))
        .route("/protected", get(protected))
        .route("/authorize", post(authorize))
        .route("/register", post(register))
        .route("/:id", get(read_todo).delete(delete_todo))
        .route("/:id/complete", put(complete_todo))
        .layer(Extension(sqlite_repo::init_db().await))
```

### A service

In Axum, a service is a function that is launched once you hit the corresponding endpoint. A service is just a normal rust function, without any macro. The only prerequisite is that the function return something that implement the `IntoResponse` trait. Fortunately, Axum already implement this trait for most of the things that you will want to send back. It is even implemented for and `Result`, which let use it directly to return the state of the request.

```rust
// A service that return a result to say if the todo to delete what found
pub async fn delete_todo(
    Path(id): Path<Uuid>,
    Extension(db): Extension<Db>,
) -> Result<(), StatusCode> {
    if delete_todo_db(&db, &id).await {
        Ok(())
    } else {
        Err(StatusCode::NOT_FOUND)
    }
}
```

### An extractor

Extractors are what Axum use to extract information from the requests that a service receive. For example, `Path(id): Path<Uuid>` that you can see above extract a Uuid from the path. If you derive the `Deserialise` trait from [serde](https://crates.io/crates/serde), you can use `Json(payload) : Json<User>` to extract a user from the JSON body.

As you can see, those extractors use types, which means that check are made on the deserialization. For example the path extractor above will reject the request if the number given is not a valid Uuid. This rejection is made before the service actually does any work, which make it way faster. The other strong point of extractors is that the deserialization and the rejection is not made in the service function, which help to keep them short and clean, and avoid code duplication.

You can also define your own extractor, when implementing the `FromRequest` trait; for example a user agent extractor:

```rust
struct ExtractUserAgent(HeaderValue);

#[async_trait]
impl<B> FromRequest<B> for ExtractUserAgent
where
    B: Send,
{
    type Rejection = (StatusCode, &'static str);

    async fn from_request(req: &mut RequestParts<B>) -> Result<Self, Self::Rejection> {
        if let Some(user_agent) = req.headers().get(USER_AGENT) {
            Ok(ExtractUserAgent(user_agent.clone()))
        } else {
            Err((StatusCode::BAD_REQUEST, "`User-Agent` header is missing"))
        }
    }
}
```

You can even use the riche rust type system to indicate if something is always needed, by putting it in an `Option`. For exemple, if the page is the argument of an URL query,

```rust
Query(page): Query<Option<u32>>
```