## Tech Resume - effortlessly make a job-worthy resume

### Introducing  [Tech Resume](https://main.doo2i0aqct9l.amplifyapp.com/) 

A resume builder which helps you to effortlessly make a job-worthy resume. Easy to use and done within minutes - try now for free!

Live: [https://main.doo2i0aqct9l.amplifyapp.com/](https://main.doo2i0aqct9l.amplifyapp.com/) 

Github Repository: [https://github.com/piyushsinha24/tech-resume](https://github.com/piyushsinha24/tech-resume)

%[https://vimeo.com/516308202]

### Features
- ✅ Ready-to-use templates 
- ✅ Color picker
- ✅ Download PDF 


### Technology Stack
- [React](https://reactjs.org)
- [Tailwind CSS](https://tailwindcss.com/)
- [Heroicons](https://heroicons.com/)
- [AWS Amplify](https://aws.amazon.com/amplify/)

### Inspiration

- Learning opportunity: Hackathon pushes me to explore and put all my knowledge in practice. Building something fresh or try to build something using technologies I have no experience with. I heard great things about `Tailwind CSS` and this was the right motivation to get some hands-on with it.

- Building my version to solve a well-known problem. No more writer’s block or formatting difficulties in Word. `Tech Resume` effortlessly makes a resume using pre-built templates and saves a lot of time.

### The Journey

I started building this web application last weekend. At first, I thought of having a `Templates` showcase webpage which would display the mini-avatars of different resume templates & further it would take you to the component with form & preview.

![old-layout.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614193633574/JHliQXCwv.png)

Then, I replaced the `Templates` showcase webpage with a simple option to select resume type within the form and toggle the state for resume preview accordingly. This layout aligned with the minimal approach of the application. 

![new-layout.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1614194391098/PYdR1cmca.png)

Now, I had a web page with different components & I had to find a way to download the resume preview component in the PDF-format and to properly fit in most commonly ISO paper size i.e. A4. At first, I made use of `html2canvas` & `jsPDF` to export the component as PDF and it did worked. 

```
const input = document.getElementById('divToPrint');
    html2canvas(input)
      .then((canvas) => {
        const imgData = canvas.toDataURL('image/png');
        const pdf = new jsPDF();
        pdf.addImage(imgData, 'JPEG', 0, 0);
        pdf.save("download.pdf");
      });
```
But the text wasn't `selectable` and it concerned me. This is the most important step in making your document readable by assistive technology. If your textual content is encoded in images in your PDF, then screen readers cannot process it. Then, I thought of the most easiest way of doing that: `window.print()`. But it will print the entire webpage right? There's a way to apply different styles specifically for `print`.

```
@media print { ... }

```
 For Tailwind CSS, all you need to do is add a `print` screen under `theme.extend.screens`:
```
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      screens: {
        'print': {'raw': 'print'},
      }
    }
  }
}
```

Then you can use classes like `print:text-black` to specify styles that should only be applied when someone tries to print the page you're working on:
```
<div class="text-gray-700 print:text-black">
  <!-- ... -->
</div>
```
So, I styled the resume preview according to A4 paper size and used `print:hidden` class to make the other components invisible. Lastly, I deployed my `React` application on `AWS Amplify` which was a seamless experience. 

### Conclusion

I would like to thank Hashnode Team for coming up with back to back hackathons. It really motivates me to keep learning & building projects. Happy Coding!
