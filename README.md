# Getting Started using themes with Hugo

Hello, here we are going to get started using hugo, a static site generator and the [devcows universal theme](https://devcows.github.io/hugo-universal-theme/). With this we will create a simple website that we will customize for a local business.

Create a new hugo site after installing hugo with

```shell
hugo new site beauty-blog
```

We'll go into the newly created site's root directory

```shell
cd beauty-blog
```

From here we can initilize a git repository

```shell
git init
```

Adding the theme as a submodule to our repository, and save in the `themes` directory at the root of our project

```shell
git submodule add https://github.com/devcows/hugo-universal-theme.git themes/hugo-universal-theme
```

## Copying the Config.toml file

copy the config.toml file and remove the hugo.toml file that came with the starter. From the root of your project execute the following command.

```shell
 cp .\themes\hugo-universal-theme\exampleSite\config.toml config.toml
```

We'll continue to change config.toml so that it fits to our linking. Let's update the following in config.toml

- about us
- the logo
- the address

## Changing the From Our blog section

Lets go ahead and change the wording in the from our blog section. The theme calls this the `Recent Posts` section. Lets change this in config.toml. Look for `[params.recent_posts]`.

Lets change it to something like

```toml
[params.recent_posts]
    enable = true
    title = "Get To Know Us"
    subtitle = "Discover a sanctuary where beauty meets well-being at Beauty Pointe. We go beyond the surface, offering a unique blend of pampering and holistic care. Our skilled team is dedicated to enhancing your natural radiance with affordable luxury. Come, explore Beauty Pointe, and let us redefine beauty as a journey of self-love and timeless transformation. Here's a few articles about us and what we are about."
    hide_summary = true
```

## Adding Content

Next we will add some content to our site so that it is ready to view.

```shell
hugo new content content/blog/beautiful.md
```

You'll notice that it only shows up in a few places this is because we need to add another file to list the blog page to let hugo know that at this route should list the pages beneath it. `_index.md`, be sure to remove the `draft`. This draft shows up because we have an archetypes file that defines what frontmatter should each content start with.

## Adding more content

We are going to add more content to fill out the blog space, we'll remove the modify the page and add more content. We'll also add some images to make our blog more engaging.

The site uses banners for each post, each photo is used in both the blog list page and the landing page of the site. We are going to places these photos in the [static](https://gohugo.io/content-management/static-files/) folder of the site.

## Adding Services to the carousel

next we will add services to the carousel. The carousel is made for the landing page of the site usign the data folder. It looks specifically for a directory named `carousel` inside the data folder it expects a yaml file to be present while being created.

I took an example of one here. Notice how im referencing an img/mani.jpg. this is a file inside my static files. I also use this file in one of the blog posts. The `>` in the description is a [Folded Block Scalar](https://yaml-multiline.info/#:~:text=Block%20Style%20Indicator,also%20not%20folded), in yaml. It allows us to ignore new lines here so we can make it easier to read.

You can see that in the description we are passing in some html. This is ok because in the layout file the contents of the description gets passed into the safeHTML processor.

```yaml
weight: 1
title: "Manicures"
description: >
  <ul class="list-style-none">
    <li>Pamper your hands with Beauty Pointe's manicures – a blend of style and relaxation. Choose from classic to trendy, ensuring both beauty and health. Treat your hands right – book a manicure now</li>
  </ul>
image: "img/mani.jpg"
```

## Creating a contact page

Next lets create a contact page. This theme has a few things wired up for it and it works on the page containing an id param with `contact` as the id.

```shell
hugo new content content/contact.md
```

Here is the content of the contact.md file.

```yaml
---
title: "Contact"
date: 2023-11-18T05:00:59-05:00
id: "contact"
---
```

You see that there is a contact form, the address that we filled in earlier. And a rendered google map page.

## Creating a google maps api key

If you want to render a map at the bottom you'll need a google account with an API Key. Go through and add your api key and change the lat long to point to the appropriate place. You'll need to add your own billing information, and Google will give you $200 of credit monthly for testing.

## Creating a formspree account

Open in a new tab here in incognito

This theme uses fromspree to submit the form to an endpoint. It is really nice and captures the contents of the form. You have up to 50 forms to post to.

## Changing the font

We'll override the css with our own css

Lets create our own in the `static` folder, create a new directory `css` and inside it create a `custom.css` file.

Paste in the following css which overides some of the styling for the classes that we have

```css
#blog-post .comment .reply {
    font-family: "Montserrat";
}

#productMain .sizes a {
    font-family: "Montserrat";
}

.breadcrumb {
    font-family: "Montserrat";
}

/* scaffolding */
body {
    font-family: "Montserrat";
}

/* labels */
.label {
    font-family: "Montserrat";
}
```

## Changing the Nav

Last thing that we'll do is change the font on our logo, the theme looks like it does not have a style for the nav, so we'll override that by creating a copy of the nav and making our own changes.

```shell
mkdir layouts\partials
```

Then we copy the theme's file into that folder

```shell
cp .\themes\hugo-universal-theme\layouts\partials\nav.html layouts\partials\nav.html
```

Next we'll look for the `h4` tag that contains the logo text and we'll modify the logo a bit.

```html
          <h4 class="logo-text">{{ .Site.Params.logo_text }}</h4>
```

Then in custom.css we'll change the font for that class

```css
.logo-text {
    font-family: "Dancing Script";
    font-size: xx-large;
}
```

You'll notice that it doesn't work, this is beause we need to override another file in the theme. `headers.html`. Lets do that by copying that file into our `layouts\partials` folder and

```shell
cp .\themes\hugo-universal-theme\layouts\partials\headers.html layouts\partials\headers.html
```

We'll replace what is in the fonts section with the following code:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href='//fonts.googleapis.com/css?family=Roboto:400,100,100italic,300,300italic,500,700,800' rel='stylesheet' type='text/css'>
<link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Montserrat:wght@400;500;600&display=swap" rel="stylesheet">

```

## Conclusion

What did we just do, we got familiar with hugo, the static site generator. We the the following:

- took a theme installed it as a submodule
- made it our own by adding our own content and styling,
- some slight modifications to the theme itself.

There are even more things we can change:

- we could upgrade this to bootstrap v5, its currently on v3.
- change more of the styling
- adding clients and testimonials theres a lot more to the theme!
