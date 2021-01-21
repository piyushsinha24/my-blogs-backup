## The Shadow DOM

In the past few years, you may have heard of terms like *shadow DOM* and *virtual DOM*. These, although of course related to the original DOM, refer to very different concepts. In this article, I will cover what, exactly, the shadow DOM is and how we can use it.

**Shadow DOM** serves for encapsulation. Both the CSS and DOM can be encapsulated so that they are hidden from the rest of the page. What a shadow DOM does is let you create a new root node, called `#shadow-root`, that is hidden from the main document.

However, even before we jump into the concept of a shadow DOM, let's try to look at what a normal DOM looks like. Any page with a DOM follows a tree structure. Here I have the DOM structure of a very simple page:


![Screenshot 2020-12-21 at 9.20.53 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1608565919129/pxUJEis55.png)

In the above image, `#document` is the root node for this page. 


> 
You can find out the root node of a page by typing document.querySelector('html').getRootNode() in the browser console.

If you try to get the child nodes of an HTML tag using `document.querySelector('html').childNodes` in the browser console, then you can see the following screenshot:

![Screenshot 2020-12-21 at 9.21.13 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1608565970733/Iy0eg_1rp.png)

This shows that the DOM follows a tree structure. You can go deeper into these nodes by clicking on the arrow right next to the name of the node. And just like how I have shown in the screenshot, anyone can go into a particular node by expanding it and change these values. In order to achieve the encapsulation, the concept of a shadow DOM was invented.

For example, let's say that instead of having text inside the `<p> tag` in the preceding example, we have a shadow host that is attached to a shadow root. The code looks like this:


```
//index.html
<!DOCTYPE html>
<html>
    <head>
        <title>Light Dom</title>
    </head>
    <body>
        <h1>This is a heading</h1>
        <p></p>
        <script src="script.js"></script>
    </body>
</html>
``` 

```
//script.js
let shadowRootEl = document.querySelector('p');
let shadowRoot   = shadowRootEl.attachShadow({mode: 'open'});
let newEl        = document.createElement('span');

newEl.innerText = "This is span tag";

shadowRoot.appendChild(newEl);
```

The call to `elem.attachShadow({mode: …})` creates a shadow tree.
The mode option sets the encapsulation level. It must have any of two values:


- open – the shadow root is available as `elem.shadowRoot`.
Any code is able to access the shadow tree of elem.

- closed – `elem.shadowRoot` is always null.

The shadow root, returned by `attachShadow`, is like an element: we can use innerHTML or DOM methods, such as append, to populate it.

The updated DOM structure:

![Screenshot 2020-12-21 at 10.28.09 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1608569909875/0juPYkeev.png)

What you see under `#shadow-root` is called `shadow DOM`. Furthermore, if you tried to get the child nodes of this `<p> tag`, you would see something like this:

![Screenshot 2020-12-21 at 10.32.53 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1608570200627/lYXi_Os6X.png)

Notice that even though the `<span> tag` is present inside the `<p> tag`, the `#shadow-root` does not let JavaScript APIs touch it. This is how the shadow DOM encapsulates code inside itself.

### Conclusion

Shadow DOM is a way to create a component-local DOM. The elements defined inside it are invisible to JavaScript selectors from the main document. To get elements in shadow tree, we must query from inside the tree. But unlike the DOM, the shadow DOM is not based on a full, standalone document. A shadow DOM, as it's name suggests, is always attached to an element within a regular DOM. Without the DOM, a shadow DOM doesn't exist.

I hope you've been enjoying my articles. If you found them useful, consider buying me a coffee! I would really appreciate it.

%%[buycoffee]