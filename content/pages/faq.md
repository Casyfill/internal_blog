title: FAQ
author: Philipp Kats
date: 2018-07-19
summary: Short description of blog internals, tutorial on writing, and FAQ

## 1. Quick order of adding your content

1. Fork [this repo][1] (or - better - ask me to allow you to commit yourself)
2. Create new file (I’d recommend Markdown) in _content_ folder
3. Fill in all the metadata (title, date, etc - use existing files as examples
4. Ideally, run `make html` (or `pelican`) command in the root (that will require python and pelican to be installed. This is not necessary, though
5. Use `git add .; git commit -m "my new post"; git push` to send your changes to GitHub (you can also use any Git/GitHub client)
6. If you forked, create a Push Request  (PR) to my original repository
7. At this point, I will coordinate the publication process

This is easier than it looks like; If that project will survive, there are dozens of ways this can be further streamlined.

---- 

## 2. How does this blog work

### 2.1 Static Blog
First, this blog is static: those pages are re-generated every time we add content to the blog; this means that there is no server to maintain and pay for; also, it means that website works very fast; Once built, the whole website is copied to the S3 bucket, which serves it’s content to the world; Buckets are virtually free for internal blogs.

### 2.2 Language and Stack

When we say _generated_, we mean that a specific piece of code took the templates, config files, and our dummy (more on that later) content, and converted all of the above into one website. In our case, this piece of code is called **[Pelican][2]** and written in Python; Nevertheless, it does not mean that you need to know how to use python or programming at all!

### 2.3 Source

All I’ve described in 1.2 - configuration, content, themes, etc. - is called _source_. Source files for our blog are stored in the repository on the GitHub - [here][3].  We use Github, as it allows us to keep data secure and revert all the changes, if necessary; It also allows us to collaborate - anyone can _commit_,say, new article (or change our theme, or add some plugin) to the blog, and even check if everything works fine after the change - but (at least for now), it will require admin to _merge_ commit into main version; After that, we can re-publish the content. In the future, we’ll be able to publish new content automatically on merge, after the submission of new post and approve  from the admin; Moreover, Github will store all the changes  - and corresponding discussions over the content;

### 2.4 Format

Earlier I called content “dummy” because it is not equal to the resulting articles. We don’t have to worry about technical details when we write our content: Pelican supports a few simple text formats, such as `markdown`, including R-markdown, `restructured text`, `latex` equations, and `Jupiter notebooks`; We can extend this list to any other format, if we want to. * 
Both [Markdown][4] and [restructed text][5] can be generated in any text editor - for example, sublime text, atom, or rStudio. There are plenty of good editors as well - for example, I am writing this post in Ulysses app, which synchronizes it’s content with the blog folder. 

Jupyter’s `.ipynb` are little different - they are most efficient if you have a lot of code in your post. Moreover, the whole notebook can be copied and used by the readers; In the future, we’ll add a `myBynder` feature, so that readers could spin-off notebooks and play with the code in  a virtual environment, without any installations.

As  Pelican just converts content (text) into html, it is 100% language-agnostic; We can show python, or R or any other code in the blog post.


In order to structure the content, Pelican needs us to fill certain number of `tags` for each piece of content, namely:
- title
- date
- author (or authors)
- category\* - tags

For example, those are the tags for this post:
	title: (Blog FAQ)
	author: Philipp Kats
	date: 2018-07-19
	summary: Short description of blog internals, tutorial on writing, and FAQ

Note, that instead of (or aside from) specifying your category in the post, you can create a folder in the `content` folder of the repository and add your post there - this will automatically create a corresponding category and add it to each corresponding post. Right now we have 3 categories: notebooks, stats, and presentations.

[Read more on content writing][6] 

### 2.5 Entry types

There are 2 main types of entries  - articles (blog posts) and pages. Main difference is that entries will show in chronological order on the main page; Pages, on the other side, are intended to be used for something like _about_, _contacts_, etc. - and will be shown on the menu for each page;

Both posts and articles will be generated using templates and look similar;
If we want to share some distinct content (say, a separate webpage that should not be converted to the template), it should be stored in `static` folder.

More on types [here][7]

### 2.4 No interaction

By it’s nature, static blog (or any site) can not support interaction (say, comments). However, we can use any external tool, or GitHub, as our communication layer - for example, it is easy to add a **DISQUS ** comments section for each article, if we want.




[1]:	https://github.com/Casyfill/internal_blog
[2]:	https://blog.getpelican.com/
[3]:	https://github.com/Casyfill/internal_blog
[4]:	https://www.markdowntutorial.com/
[5]:	https://thomas-cokelaer.info/tutorials/sphinx/rest_syntax.html
[6]:	http://docs.getpelican.com/en/3.6.3/content.html#writing-content
[7]:	http://docs.getpelican.com/en/3.6.3/content.html