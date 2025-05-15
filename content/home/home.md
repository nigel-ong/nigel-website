+++
title =  "Home"
type = "home"
draft = false
+++


{{< showcase-section
    title="'Sup"
    subtitle="Thanks for dropping by"
    buttonText="Email"
    description="<strong>Strong</strong>, <em>italic</em> and normal text. This comes from <a href='https://github.com/zetxek/adritian-demo/blob/main/content/home/home.md?plain=1'><code>home.md</code></a>, using the <code>showcase-section</code> <a href=''>shortcode</a>.<br/>Below you can see the social links, provided by the <code>platform-links</code> shortcode."
    imgSrc="images/showcase/showcase.png"
    imgScale="0.5"
 >}}

{{< platform-links >}}
    {{< link icon="linkedin" url="https://www.linkedin.com/in/niong" >}}
    {{< link icon="square-github" url="https://github.com/nigel-ong" >}}
{{< /platform-links >}}

{{< /showcase-section >}}

{{< about-section
    title="About me"
    content="This content is using the <code>about-section</code> shortcode. <br/>You can write <code>HTML</code>, as long as you <em>wrap it</em> accordingly. "
    button_icon="icon-user"
    button_text="You can edit the text, link and icon"
    button_url="https://www.google.com"
    imgSrc="images/about/user-picture.jpg"
    imgScale="0.5"
    centered="true"
 >}}

<!-- {{< experience-section
    title="My job experience (title)"
    intro_title="Intro (intro_title)"
    intro_description="Description (intro_description).<br>You can use HTML,with <strong>strong</strong> formatting, or lists <ul><li>one</li><li>two</li></ul>" 
    button1_url="https://example.com"
    button1_text="Visit Example"
    button1_icon="icon-globe"
    button2_text="All experience"
    button2_url="/experience"
    button3_text="Button #3"
    button3_url="/experience"
>}} -->

{{< experience-list
    title="Experience"
    padding="false" >}}



{{< client-and-work-section
    title="Projects at BCIT" >}} 
<!-- 
{{< testimonial-section
    title="What they say about me" >}} -->
{{< education-list
    title="Education" >}}

{{< spacer size="large" >}}

## Extra home content

Additional content added after the `section` blocks, in the `home.md` file. 

Here you could freestyle, add other shortcodes, ...  Or just let the content empty, and rely on the shortcode sections alone.

<!-- {{< spacer size="small" >}} -->
<!-- {{< spacer size="large" >}} -->

{{< text-section
title="Extra (centered) content"
centered="true"
>}}

You can also use the `text-section` shortcode to add centered texts

{{< /text-section >}}
