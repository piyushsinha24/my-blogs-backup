## Page Speed Optimization

**Page Speed** is the amount of time it takes to completely load content on your webpage.

A user goes to a browser, puts in your website, and there is a DNS request. This points at your domain name provider, so maybe GoDaddy, and this points to your server where your files are located. So the DOM starts to load all of your HTML, CSS, and JavaScript. But very rarely does this one pull all of the needed scripts or needed code to load a web page. 

Typically the DOM will need to request additional resources from your server to make everything happen, and this is where things start to really slow down your site. 

### Why you should care about page speed?

- Page speed is a critical factor when it comes to ranking your website higher on Google’s search engine results.

- Slower the site, the chance of someone bouncing from your site increases dramatically:

![Screenshot 2021-01-13 at 2.23.40 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610528036223/gcdIzUhOb.png)

- Better user experience.

### Page Speed Optimization

Here are some of the many ways to increase your page speed:


**1. Use a CDN**

Content distribution networks (CDNs), also called content delivery networks, are networks of servers that are used to distribute the load of delivering content. It effectively gives you access to hundreds of little servers across the world that host a copy of your site for you, massively reducing the time it takes to fetch your site. If you're not using a CDN, every request to your website (including images, CSS and JavaScript), gets routed across the world, slowly, to your server.

**2. Optimize Images**

Be sure that your images are no larger than they need to be, that they are in the right file format (PNGs are generally better for graphics with fewer than 16 colours while JPEGs are generally better for photographs) and that they are compressed for the web.

**3. Browser Caching**

Browsers cache a lot of information (stylesheets, images, JavaScript files, and more) so that when a visitor comes back to your site, the browser doesn't have to reload the entire page. This speeds up page load for the existing users.

**4. Reduce Redirects**

Redirects slow down your site considerably. Each time a page redirects to another page, your visitor faces additional time waiting for the HTTP request-response cycle to complete. Instead of having special subdomain for mobile users, use responsive CSS and serve your website from one domain.

Some redirects are unavoidable, such as www -> root domain or root domain -> www, but the majority of your traffic shouldn't be experiencing a redirect to view your site.

**5. Minify Resources (HTML, CSS and JavaScript)**

Minifying your code (include removing spaces, commas, and other unnecessary characters), you can dramatically increase your page speed. Also remove code comments, formatting, and unused code. 

You can use tools like  [HTMLMinifier](https://github.com/kangax/html-minifier ), [CSSNano](https://github.com/ben-eb/cssnano) and [UglifyJS](https://github.com/ben-eb/cssnano) to minify your resource files. 

**6. Remove Render-Blocking JavaScript**

Before the browser can render a page it has to build the DOM tree by parsing the HTML markup. During this process, whenever the parser encounters a script it has to stop and execute it before it can continue parsing the HTML. In the case of an external script, the parser is also forced to wait for the resource to download, which may incur one or more network roundtrips and delay the time to first render of the page.

Scripts that are necessary to render page content can be inlined to avoid extra network requests, however, the inlined content needs to be small and must execute quickly to deliver a good performance. 

```
<html>
  <head>
    <script type="text/javascript">
      /* contents of a small JavaScript file */
    </script>
  </head>
  <body>
    <div>
      Hello, world!
    </div>
  </body>
</html>
```

Scripts that are not critical to initial render should be made asynchronous or deferred until after the first render. To prevent JavaScript from blocking the parser use the HTML `async` attribute on external scripts:

```
<script async src="my.js">
```

### Conclusion

When you’ve spent countless days, weeks, and months building a new website, you want it to be perfect. I would recommend to regularly track your site's speed
using tools like [WebPageTest](https://webpagetest.org) & improve it using the above points.

### References

- [Make The Web Faster](https://developers.google.com/speed)


 