## Building chatbots with DialogFlow, NodeJS, and React

To us humans, conversation is second-nature; it comes naturally to us, but the same can’t be said for bots. Even a simple question like, *“How was your day?”*, could be rephrased several ways (for example, *“How’s it going?”*, *“How are you?”*), none of which a bot can ordinarily understand.

We can interpret the intent behind the question, but a lot goes into building the logic to facilitate a smarter conversation with a bot, and for most developers, coding it from scratch isn't feasible.

Well, fortunately, there's something called **NLU (natural-language understanding)**, which enables better human-computer conversation – in other words, a smart chatbot that utilizes machine learning and other technologies in order to better understand human interactions.

An NLU algorithm doesn't just recognize text but also interprets the intent behind it. Hence, it’s an ideal algorithm for chatbots. Read more about NLU [here](https://research.aimultiple.com/nlu/).

And this is where the purpose of this article comes in. We will build our chatbot using Google's NLU platform, **DialogFlow**. Read on to learn more about DialogFlow, and how to integrate it into a React application with this follow-along tutorial.

# Getting started with DialogFlow

In simple terms, DialogFlow is an end-to-end tool powered by NLU to design and integrate chatbots into our interfaces. It translates *natural language* into *machine-readable* data using machine learning (ML) models trained by the language we provide. How does it work? Let's learn while we build our chatbot.

Open the [DialogFlow](https://dialogflow.cloud.google.com/#/login) console and log in with your Google account. Upon successful login, we see the following interface:

![dialogflow-console](https://cdn.hashnode.com/res/hashnode/image/upload/v1664627203361/wq5PQd4da.png align="left")


The first thing that may catch your attention is the** Create Agent** option. 

## What is an Agent?

Nothing fancy! The chatbot itself is an agent. Collecting the user's query, acting on it, and finally sending a response are all handled by our agent. 

Let's create our chatbot; how about a bot for a coffee shop, for our example? No prizes for guessing my coffee shop inspiration.

![create-agent](https://cdn.hashnode.com/res/hashnode/image/upload/v1664627861437/snMm9MXLK.png align="left")

This is how the console should look now:

![console-agent](https://cdn.hashnode.com/res/hashnode/image/upload/v1664627953964/tL4PR5rTf.png align="left")

Now, more jargon has appeared on the screen – **Intents**. 

## What are Intents?

The console says, *"Intents are mappings between a user's queries and actions fulfilled by your software"*. So, what does this mean?

Let me explain: in the case of our bot, we expect to receive queries like, *"I would like a cappuccino"* and, *"When does the shop open?"*, etc. We can categorise these queries into user intentions such as *"Take Order"*, *"Timings"*, etc. To handle them, we define these categories as intents in our agent.

We also see that our agent comes with two default intents; **Default Welcome Intent** and **Default Fallback Intent**. Let's explore them in a little more detail:

![default-welcome-intent](https://cdn.hashnode.com/res/hashnode/image/upload/v1664628569744/VpdXVh4hm.png align="left")

Quite a bit of jargon here; let's go through them one by one:

### Contexts

In a human conversation, to understand phrases, we usually need some context. Likewise, with the bot, an intent needs to know the context of the query. To make this possible, we connect one or more intents via contexts. We will learn more about this later in this article.

### Training phrases

These are example phrases to train and help our agent match queries with the right intent. More phrases and variations will improve the accuracy of intent matching.

By seeing the default list of phrases, it's clear that this intent is used to greet our users.

### Events

We just learned the agent looks for training phrases to trigger intents. However, intents can also be triggered by events. There are two types of events:

- **Platform events**: These are provided by the platform itself and occur when platform-specific events occur (e.g., a Welcome event).

- **Custom events**: These are defined by us (e.g., the response from an API call made by us).

### Actions and parameters

Once the query is matched with the right intent, next comes **action **on it. To act, sometimes we need to extract some data from the query. To extract, we define **parameters **with entity types. Take this for example: *"Is the coffee shop open today?"*; the parameter to extract here is *today*, which is critical info needed to perform some logic and respond accordingly. We will learn more about this later in this tutorial.

### Responses

Response to the user. Here, we see static phrases, but they can be dynamic, too, if we make use of *parameters *or *fulfillment*.

### Fulfillment

When we enable fulfillment, the agent gives a dynamic response by making an API call defined by us (e.g., if a user wants to book a table, we can check the database and respond based on availability). Again, more on this a little later.

Let's try this default intent first before going any further. On the right side, we have a **Dialogflow Simulator** to test our agent. We say something similar to the training phrases, and the agent responds from the list of responses:

![user-hello](https://cdn.hashnode.com/res/hashnode/image/upload/v1664631017775/kowZGtvv4.png align="left")

And, if we say something completely different, the agent triggers a **Default Fallback Intent**:

![fallback-intent](https://cdn.hashnode.com/res/hashnode/image/upload/v1664631149159/q_qZGF-cT.png align="left")

So, if the agent fails to find the right intent, it triggers the fallback intent. Pretty smooth, right? But, let's try to avoid it and add intents to carry out the whole conversation.

## Adding regular intents

Let's try to create this conversation shown in the diagram below:

![target-conversation](https://cdn.hashnode.com/res/hashnode/image/upload/v1664631411178/D8sLSf_bG.png align="left")

Modify the response of **Default Welcome Intent** to greet and ask for the order. Something like this; *"Greetings! What would you like to order?"*.

Create an intent to take the order, and name the intent to something simple that makes sense, like **Take Order**.

Now, we add training phrases. The user might say, *“I would like to have 2 lattes”*. Adding this to the list of training phrases is enough to get it matched with this intent. But, we also need the qualifying information; *‘2’* and *‘lattes’*. 

To extract them, we will add a couple of *parameters* within the phrase – **quantity (sys.number entity type)** and **item(sys.any entity type)**:

%[https://www.canva.com/design/DAFNzyAEw8A/watch]

Add more variations to make it better. Here’s some I added:

![training-phrases-order](https://cdn.hashnode.com/res/hashnode/image/upload/v1664634134941/lqmAPuixz.png align="left")

Once the parameter-rich phrases are added, this table is automatically filled. We can mark them as **required/optional** and set a **default** value if needed. In addition, we need to define a question as a **prompt**, to force the user to enter the required parameter, if not entered the first time. 

So, this means that if we get the first phrase from the list, *“I would like to order”*, and the **item (required)** is missing, the prompt will get triggered to ask for it. If we get the second phrase from the list; “I would like to order a mocha”, and **quantity (optional)** isn’t there, then, instead of prompting the user to enter the quantity, we will just set the default value as 1 and use that.

We can return a dynamic response using parameters to repeat the order and ask if they would like an add-on:

![dynamic-response](https://cdn.hashnode.com/res/hashnode/image/upload/v1664637774592/OD4C_3tuG.png align="left")

To do this, create an intent to handle the user’s response to the add-on question.

The response will be either *“Yes”* or *“No”*. This is the ideal scenario to introduce **follow-up intents**. As the name suggests, these are used to *follow up* with the conversation in the parent intent. 

DialogFlow provides many predefined follow-up intents for common replies like *“Yes”*, *“No”* or *“Cancel”*. Also, If you need to handle custom replies, you can create your own custom follow-ups. We’re going to use the predefined ones for this tutorial, but it’s up to you if you wish to put your own spin on it.
	
From the intent list, hover the mouse over the **TakeOrder** intent and click Add follow-up intent. Select **Yes **and the follow-up intent is created named **Take Order - yes**.

![take-order-yes](https://cdn.hashnode.com/res/hashnode/image/upload/v1664638477350/ZrqEDYjDl.png align="left")

You may have noticed there’s an **input context** added (**TakeOrder-followup**). For follow-ups to work, they need to be linked to the parent intent, and that’s where the context comes in handy.

When we create a follow-up intent, an **output context** is automatically added to the parent intent and an **input context** of the same name is added to the follow-up intent.

We don’t need to worry about the training phrases, as they are already in place. Now, add a dynamic response for this intent. As contexts are in place, we can use the parameters from the parent intent, like this:

![follow-up-yes-response](https://cdn.hashnode.com/res/hashnode/image/upload/v1664638705043/navjBieeG.png align="left")

Similarly, create a predefined follow-up intent to handle **No** in response. This one will be named **Take Order – no**. Just add a dynamic response here, as well:

![follow-up-no-response](https://cdn.hashnode.com/res/hashnode/image/upload/v1664638825197/YDxq_X-el.png align="left")

The agent is ready to carry out the conversation. We can always test it in the simulator, but let’s try something different. Click **Integrations **in the sidebar and enable **Web Demo**. Open the URL provided there and start the conversation:

%[https://www.canva.com/design/DAFNzw8H8tE/watch]

The agent is working fine as expected, but wouldn’t it be nice to show the total billing amount of the order? We can fetch the prices from a server and return the calculated amount to the user.

# Connecting with a NodeJS server

While discussing intents, we came across **Fulfillment**, which helps to return dynamic responses by making API calls defined by us. That’s what we need here to interact with our server.

Find **Fulfillment** in the sidebar and open it. DialogFlow provides two ways to use fulfillment:

- An in-line editor powered by **Google Cloud Functions**. But, to enable this, we need to add a valid billing account as this integration has charges if used beyond a certain limit.

- A webhook service

Let's build a webhook service and make it public. Before proceeding, make sure the service meets the requirements mentioned [here](https://cloud.google.com/dialogflow/es/docs/fulfillment-webhook).

## Building our server

Create a node app in a new directory and install the necessary dependencies:
```bash
mkdir centralperk-server
cd centralperk-server
npm init -y
npm i express dialogflow-fulfillment
```

Start with a basic server in index.js:
```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hi from server!");
});

app.listen(8080, () => {
  console.log("server running...");
});
```

This simply runs the server at port **8080**. Now, let’s write some code to handle webhook requests from DialogFlow:
```js
const express = require("express");
const app = express();

const { WebhookClient } = require("dialogflow-fulfillment");
const getPrice = require("./helpers");

app.get("/", (req, res) => {
  res.send("Hi from server!");
});

app.post("/", express.json(), (req, res) => {
  const agent = new WebhookClient({ request: req, response: res });

  function handleIntent(agent) {
    const intent = agent.intent;
    const item = agent.contexts[0].parameters.item;
    const quantity = agent.contexts[0].parameters.quantity;
    const billingAmount = getPrice(intent, item, quantity);

    const response =
      intent === "Take Order - yes"
        ? `Great! Your ${quantity} ${item} and cookies will be ready in no time. Please pay ${billingAmount}$.`
        : `Okay! Your ${quantity} ${item} will be ready in no time. Please pay ${billingAmount}$.`;

    agent.add(response);
  }

  const intentMap = new Map();
  intentMap.set("Take Order - yes", handleIntent);
  intentMap.set("Take Order - no", handleIntent);
  agent.handleRequest(intentMap);
});

app.listen(8080, () => {
  console.log("server running...");
});
```

Helpers to calculate the price:
```js
const priceList = {
  mocha: 5,
  latte: 7,
  cookies: 2,
};

module.exports = function (intent, item, quantity) {
  const total =
    intent === "Take Order - yes"
      ? priceList[`${item}`] * quantity + priceList["cookies"]
      : priceList[`${item}`] * quantity;

  return total;
};
```

Let’s walk through the above implementation.

The imported **WebhookClient** will handle the communication with DialogFlow’s webhook fulfillment API. When a *fulfillment-enabled* intent is matched, an HTTPS POST webhook request is sent to our server. This request is handled by `agent.handleRequest(intentMap)`. It takes in a map of handlers and each handler is a function callback. The one defined here extracts all the needed information from the passed instance, calculates the billing amount, and then finally returns the dynamic response.

## Making the server public

Using **ngrok** is the fastest and easiest way to put the server on the Internet. Follow the steps [here](https://dashboard.ngrok.com/get-started/setup)for quick setup and installation. Once done with the steps, run the following command:
```bash
ngrok http 8080
```

And the secured public URL is ready in no time:

![ngrok-url](https://cdn.hashnode.com/res/hashnode/image/upload/v1664649417569/9xJOmCS_Z.png align="left")

> Note: Remember to keep the local server up and running

## Enabling the webhook

Go ahead and enable the webhook in the Fulfillment window and enter the secured public URL. In addition, bear in mind that webhook calls need to be enabled for both follow-up intents.

All looks good. Let’s test it now:

%[https://www.canva.com/design/DAFN0gJ1PtE/watch]

Our chatbot is all set up!

# Integrating the DialogFlow chatbot into a React App

There are many ways to integrate the chatbot into a React app:

- Build the chat widget in React from scratch. Handle the state of incoming and outgoing messages using a library like Redux and modify the Node server to handle calls from the React app as well as send them to DialogFlow. This does sound interesting, but is a lot to cover and is outside the scope of this particular article.

- Using [Kommunicate](https://www.kommunicate.io/)to rather more effortlessly integrate the DialogFlow chatbot into React app.

In this blog tutorial, we’re going with the Kommunicate option.

## DialogFlow ES: Kommunicate integration

Follow these steps:

Go for Free Trial and sign up with your Google account.

Click **Bot Integrations** and select **DialogFlow ES**.

Get the JSON key from the DialogFlow cloud account using the instructions mentioned in the following image:

![bot-integration-json-key](https://cdn.hashnode.com/res/hashnode/image/upload/v1664650033716/S6J0NIbG8.png align="left")

Next, we get to choose a customized avatar:

![choose-avatar](https://cdn.hashnode.com/res/hashnode/image/upload/v1664650086076/5vF9rNrT3.png align="left")

And that’s all we need to do for DialogFlow ES and Kommunicate integration!

![kommunicate-success](https://cdn.hashnode.com/res/hashnode/image/upload/v1664650132043/KIxJw8Brq.png align="left")

## Integrate the Kommunicate chat widget into a React App

Create a chatbot component and paste the following code in `useEffect()`:

```js
function Chatbot() {
  useEffect(() => {
    (function (d, m) {
      var kommunicateSettings = {
        appId: "<YOUR APP_ID>",
        popupWidget: true,
        automaticChatOpenOnNavigation: true,
      };
      var s = document.createElement("script");
      s.type = "text/javascript";
      s.async = true;
      s.src = "https://widget.kommunicate.io/v2/kommunicate.app";
      var h = document.getElementsByTagName("head")[0];
      h.appendChild(s);
      window.kommunicate = m;
      m._globals = kommunicateSettings;
    })(document, window.kommunicate || {});
  }, []);
  return <div></div>;
}

export default Chatbot;
```

> Remember to replace the placeholder with your appId.

Finally, import it into the App component and run the app locally to test the integration:

![local-react-bot](https://cdn.hashnode.com/res/hashnode/image/upload/v1664650438811/usQYACzt6.png align="left")

And just like that, we have added our chatbot to our React app. Visit this dashboard to add more customizations to things like *color*, *icons*, and *notification *sounds, etc.

# Conclusion

That’s all for this blog! We learned several aspects of chatbot development; from building it with DialogFlow, connecting with the NodeJS server, to finally Integrating it into React app. I hope it made sense to you and you were able to follow along with this tutorial with ease.

If you have any questions, you can leave them in the comments and I’ll be happy to answer them. Feel free to reach out to me on [LinkedIn](https://www.linkedin.com/in/dev-ps/)or [Twitter](https://twitter.com/piyushsinhadev).
