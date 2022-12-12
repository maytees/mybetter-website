---
layout: "../../layouts/BlogPost.astro"
title: "How to create a CLI what-to-code app with Rust."
description: "This is a guide on how to create a what to code application which gives you ideas for your next awesome project via the cli"
image: "/something.jpg"
---

# How to create a what to code CLI app in Rust!

Hello and welcome to this very cool tutorial which will teach you how to create a CLI app which can give you ideas for projects to create.

### How will this work?

This app will use the _What-to-code_ API to grab random ideas and display them in your terminal. If you would like to see the full project code, [this](https://github.com/maytees/whattocode) is the Github repository _(which is a little bit different)_ for the completed project.

### What will we use?

We will be using the Rust programming language for this project, and for the libs, we will be using the following:

- `reqwest`
  - `Version 0.11`
  - `Features:`
    - `json`
    - `blocking`
- `tokio`
  - `Version 1`
  - `features:`
    - `full`
- `serde`
  - `Version 1.0`
  - `features`
    - `Derive`
- `question`
  - `Version 0.2.2`

Here is a text block with the full cargo.toml dependencies list:

```toml
[dependencies]
reqwest = {version = "0.11", features = ["json", "blocking"]}
tokio = { version = "1", features = ["full"] }
serde = { version = "1.0", features = ["derive"] }
question = "0.2.2"
```

### Now we start!

First, we need to create the Rust project, to do this, go to your terminal, and run the following command: `cargo new what-to-code`. After this, we need to cd into the folder (which will be named `what-to-code`), and open it with your favorite code editor, whether it may be Vim, Neovim, VSCode, Helix, etc. I prefer to use Helix, so I will run `helix .`.

Head over to the [main.rs](http://main.rs) file, and add the following _imports_:

```rust
use serde::Deserialize; // Used to put the output of the API into a struct

// CLI question library, used just for ease
extern crate question;
use question::*;
```

Now that we have entered in the imports, lets move on to the main function:

```rust
#[tokio::main] // This is used to synchronize the main function, allowing us to use async
async fn main() -> Result<(), Box<dyn std::error::Error>> { // The Result is required by tokio, becuase this main function is async

	Ok(())
}
```

### The What To Code API -

We need a random idea from the what to code API. Using [this](https://what-to-code.com/api/ideas/random) API link, we can get a JSON object containing the title, description, tags, id, and likes of an idea. The only 3 parts of this data that we will use will be the title, description, and likes of the idea. We are going to be using the reqwest lib/crate to grab the data from the URL via a GET request, and giving the data to serde, which will store the data into a struct.

_A note that you should keep in mind when using this if you are planning to or are already building it is that this API sometimes deletes all of it’s ideas for some reason, so it may give you many duplicate ideas, this is simply because the API just doesn’t have lots of ideas to pass around. If you want, you are able to go on the website, [https://what-to-code.com](https://what-to-code.com/), and add your own ideas 😎_

Lets do what I just explained above -

First, we will need to create a Struct, which will contain the relevant data that we need, to do this, we just have to give it the title, description and likes attributes, along with the Deserialize trait (via #[derive]).

```rust
#[derive(Deserialize)]
pub struct Idea {
	title: String,
	description: String,
	likes: usize,
}
```

Next, we need to create the main loop of the CLI, which contains code to get the data from the API:

```rust
#[tokio::main] // This is used to synchronize the main function, allowing us to use async
async fn main() -> Result<(), Box<dyn std::error::Error>> { // The Result is required by tokio, becuase this main function is async
	loop {
		let client = reqwest::Client::builder().build()?; // This will create a reqwest instance, via the builder provided to us

		let res = client
			.get("https://what-to-code.com/api/ideas/random")
			.send()
			.await?;

		let json_res = res.json::<Idea>().await?;
	}
	Ok(())
}
```

Lets explain the code above block by block:

- The loop block is very self explanatory, it creates a loop which will loop indefinitely.
- The client variable is simply creating an instance of reqwest, just like the comment suggests.
- The res variable is telling reqwest to **_get_** the data from the specified URL, then telling reqwest to send the get reqwest, _then_ awaiting a response.
  - Keep in mind that this is returning a Result<>, which is why we have the `?` there. (The `?` operator just simplifies the handling of a statement which returns a result)
- The json_res variable takes in the result and parses it using the json function. The JSON function takes in a generic which tells the parser what to parse the data into.. in this case, we are parsing the data into the `Idea` struct, which we created earlier. We use `await?` here again, because the json function is asynchronous.

Now, we are done with the part which gets the data from the API, lets move on to the CLI part.

### The CLI part -

In this part of the guide, we will implement the part which the user communicates with. For this, we will be using the question crate, which simply asks a question, waits for an input, and tells us the input, then we act upon it. (I hope that explanation was not confusing). But before we even ask a question, we must first print out the idea.

Here is what we have:

```rust
#[tokio::main] // This is used to synchronize the main function, allowing us to use async
async fn main() -> Result<(), Box<dyn std::error::Error>> { // The Result is required by tokio, becuase this main function is async
	loop {
		let client = reqwest::Client::builder().build()?; // This will create a reqwest instance, via the builder provided to us

		let res = client
			.get("https://what-to-code.com/api/ideas/random")
			.send()
			.await?;

		let json_res = res.json::<Idea>().await?;

		// Prints out the idea for the user to view
		println!(
				"\n\nHere is an idea: \n {} \n{}\nThis idea has {} likes\n\n",
				json_res.title, json_res.description, json_res.likes
		);

		let q = Question::new("Would you like a new idea?")
				.default(Answer::YES)
				.show_defaults()
				.confirm();

		if q == Answer::YES {
			continue;
		} else {
			break;
		}
	}
	Ok(())
}
```

Let me explain what is going on here:

1. We are first printing out the idea that was just given to us by the API. All the `\n` break characters that you see are simply there for formatting, which you can modify however you would like of course.
2. Then, we are creating a variable, named q, which will create a question with the question being → **“Would you like a new idea?”.**
3. As configuration to this question, we are telling the question library to make the default answer YES, meaning that if we do not respond with y, or n, but rather just click enter, the answer will default to YES. Another part of the configuration is show_defaults(), which just capitalized the default answer (being Y). and finally, we are confirming the question, which simply just collects all the config and asks the question.
4. Finally, we are checking if the answer is yes, if the answer is yes, we are _continuing_ the loop, otherwise, we are _breaking_ out of the loop, closing our program.

And just like that, we are done!

You can go back to your terminal, and run the following command to test out your what to code program: `cargo run`

Here is the full code:

```rust
use serde::Deserialize; // Used to put the output of the API into a struct

// CLI question library, used just for ease
extern crate question;
use question::*;

#[derive(Deserialize, Debug)]
pub struct Idea {
    title: String,
    description: String,
    likes: usize,
}

#[tokio::main] // This is used to synchronize the main function, allowing us to use async
async fn main() -> Result<(), Box<dyn std::error::Error>> { // The Result is required by tokio, becuase this main function is async
	loop {
		let client = reqwest::Client::builder().build()?; // This will create a reqwest instance, via the builder provided to us

		let res = client
			.get("https://what-to-code.com/api/ideas/random")
			.send()
			.await?;

		let json_res = res.json::<Idea>().await?;

		// Prints out the idea for the user to view
		println!(
				"\n\nHere is an idea: \n {} \n{}\nThis idea has {} likes\n\n",
				json_res.title, json_res.description, json_res.likes
		);

		let q = Question::new("Would you like a new idea?")
				.default(Answer::YES)
				.show_defaults()
				.confirm();

		if q == Answer::YES {
			continue;
		} else {
			break;
		}
	}
	Ok(())
}
```
