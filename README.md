# TNW DevLabs Blog

Feel the itch to blog? Work for The Network Inc (you ought to if you are reading this)? Well, what are you waiting for, The Network Inc. uses the blog to attract other top talent in the greater Atlanta area and build recognition within the community, so get posting!

## Adding A New Blog Post
You can add a new blog post by adding a page to the blog repository. The repo name is tnwinc.github.com and the blog entry directory is named "_posts". Simply add a new markdown file to the pages following the naming conventions yyyy-mm-dd-hyphenated-title.md (there are many posts there, just follow suit).

## Required Blog Text
The headers that are added to the beginning give the parser all the information it needs for displaying the blog post appropriately on the web site. The headers needed at the top of the blog are as follows:

    ---
    title: "Your blog title goes here"
    layout: post
    tags: [tag1, tag2]
    published: false
    author: Your Name
    mail: "YourEmailAddress@tnwinc.com"
    summary: The text summary of your blog post goes here!
    ---
    
## Publishing Your Blog Post
Once your blog post has been reviewed and tweaked in such a way that you are ready for it to go live, simply change the published attribute in the header to true.
