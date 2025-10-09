# Creating a canned response machine

Imagine you receive the following text:

```md
Why is there no search available!?
```

Your customer found out there is something missing that they want.
And what they want is not available - however simple it appears to them - there will be engineering work to do.

## Create a one-click response

Whether it's search, another feature or maybe a bug, we need to inform and respond to the customer that this we acknowledge their concern.

[Open HAI](https://thomasjwilliam.github.io/hai-pub/#/compose)

In the library, click the link `Save a snippet of text` and provide (or copy) the following information:

Title:
```
Feature Request
```
Trigger:
```
fr
```
Text #1:
```
Hello,

Thank you for getting in touch.

That feature is currently not available.
But we will definitely put it on our roadmap.

Kind regards,
A Human
```

Click `SAVE` and confirm the new snippet with `OK`. You now have your first snippet in your library.  
Click the snippet in the library and see its text injected into the notepad.

## Breaking down a response into reusable snippets

Looking closer at the text, it can be broken down into 4 different snippets.
Three of them are generic (i.e. greeting the customer, thanking them, and signing off), with
the other one specific to the context, i.e. the fact that this is a feature request.

Similar to [the rule of three](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)) in programming,
you ask yourself "will I be able to reuse this snippet of text again in different contexts". If the answer is yes,
break it down:

```text
// highlight-next-line
Hello, (greeting)

// highlight-next-line
Thank you for getting in touch. (thanking)

// highlight-start
That feature is currently not available. (feature request)
But we will definitely put it on our roadmap.
// highlight-end

// highlight-start
Kind regards, (signature)
A Human
// highlight-end
```

Let's create the individual snippets for reuse.

Start with the **greeting**.  
In the library, click the `+` button, select `SNIPPET` and provide the following information:

Title:
```
Greeting
```
Trigger:
```
hai
```
Text #1:
```
Hello,
```

Click `SAVE` and confirm with `OK`.

Let's create the next snippet, **thanking to get in touch**.  
In the library, click the `+` button, select `SNIPPET` and provide the following information:

Title:
```
Thanking
```
Trigger:

Don't provide a trigger this time.  
Click the `$` sign to automatically generate the trigger from the title.

Text #1:
```
Thank you for getting in touch.
```

Click `SAVE` and confirm with `OK`.

From this point onwards, to keep this and other tutorials easier to read, a snippet will be represented
in the following format:
```md title="Title (trigger)"
Text
```

To finish breaking down the text, create the following two last snippets:

```md title="Feature Request (fr)"
That feature is currently not available.
But we will definitely put it on our roadmap.
```

```md title="Signature (sign)"
Kind regards,
A Human
```

If you completed these steps, you should have the following snippets in your library:
- Feature request
- Greeting
- Thanking
- Feature request
- Signature

This library of snippets is stored locally in your browser's storage.  
You can close the window/tab and reopen it. Your library with the 5 snippets will be there.

## Interacting with the library




## Deprecated

So you respond:

```md
Hello,

Thank you for getting in touch.

That feature is currently not available.
But we will definitely put it on our roadmap.

Kind regards,
A Human
```

This is a common occurrence, responding to requests or issues that are known by you but not by the users of your
products or software.

[Open Hai](https://thomasjwilliam.github.io/hai-pub/#/compose)

Write (or copy) in the document composer.

```text
Hello,

Thank you for getting in touch.

That feature is currently not available.
But we will definitely put it on our roadmap.

Kind regards,
A Human
```

Looking at the above text, it can be broken down into 4 different snippets.
Three of them are generic (i.e. greeting the customer, thanking them, and signing off), with
the other one specific to the context, i.e. the fact that this is a feature request.

Here is the breakdown:

```text
// highlight-next-line
Hello, (greeting)

// highlight-next-line
Thank you for getting in touch. (thanking)

// highlight-start
That feature is currently not available. (feature request)
But we will definitely put it on our roadmap.
// highlight-end

// highlight-start
Kind regards, (signature)
A Human
// highlight-end
```

Let's create individual snippets from this text to reuse.  

## Creating your first snippet

First we start with the greeting.
In the library, click the link `Save a snippet of text` and provide the following information:

Title:
```
Greeting
```
Trigger:
```
hai
```
Text #1:
```
Hello,
```

Click `SAVE` and confirm the new snippet with `OK`.  
You now have your first snippet of text in your library.

Let's create the next snippet, thanking to get in touch.  
In the library, click the `+` button, select `SNIPPET` and provide the following information:

Title:
```
Thanking
```
Trigger:  

Don't write the trigger this time. Click the `$` sign to automatically generate the trigger from the title.

Text #1:
```
Thank you for getting in touch.
```

Click `SAVE` and confirm the new snippet with `OK`.  
That's the second snippet in the library.

From this point onwards, to keep this and other tutorials easier to read, a snippet will be represented as:
```md title="Title (trigger)"
Text
```

To finish breaking down the text with the last two snippets, create the following snippets,
each time with the `+` button and selecting `SNIPPET`

```md title="Feature Request (fr)"
That feature is currently not available.
But we will definitely put it on our roadmap.
```

```md title="Signature (sign)"
Kind regards,
A Human
```

If you completed these steps, you should not have the following snippets in your library:
- Greeting
- Thanking
- Feature request
- Signature
