# CGIBlog
A minimalistic blog implemented as Bash script. It uses [Perl script](https://daringfireball.net/projects/markdown/) for Markdown to HTML conversion and [Mustache](http://mustache.github.io/) templating engine with [Bash implementation](https://github.com/tests-always-included/mo) for it. The templates are referring [Bootstrap](https://getbootstrap.com/) to make it look nicer.

The project builds upon the examples I already had in the [CGI Server](https://github.com/szabogabriel/cgiserver) project.

The story is simple. I was on vacation and wanted to start documenting it in form of a blog. I had a similar implementation already ([SimpleBLOG](https://github.com/szabogabriel/SimpleBLOG)), but I couldn't get it up and running with the NGinx reverse proxy I have on my server. It was especially challanging, since I was doing all this via my smartphone using an SSH client to connect to the cloud server. So I decided to use my CGI Server and try to change the demos there to achieve something similar to a blog. The shell scripts were written directly on the server via the SSH client.

I know this implementation isn't effective and it doesn't perform well. It is however the one I could come up with given the circumstances. On the other hand, it is funny to see, how the bots are trying to guess the technology if you set up a page like this. With no database, no login and no information whatsoever about the page, I don't think it is at a great risk.

## Technical overview

The script is simple, and expects environment vaeiables to be set. Especially the QUERY_STRING. It is used to request pages and images.

It expects basic Linux commands to be available (ls, awk, pwd, dd etc).

The script responses with the Content-type HTTP header set accordingly (text/html or image/jpeg).

The page does not set or use any cookies. Nor does it collect any data.

## Functional overview

In essence, the blog is a single page application, where everything is provided by the simple `index` script.

The menu items are a list the folder names holding the blog content. The images folder is ignored.

If no query string is provided, it will render the `index.md` file.

If the ABOUT query string attribute is set, it will rende rthe `about.md`.

If DIR query string attribute is set, it will list the content of the folder set. It only puts out the name of the files present.

If both the DIR and the ENTRY attributes are set, it will render the given entry. If it is a Markdown file, it will be converted to HTML, otherwise it will be oresented directly.

If the IMAGE attribute is set, then the file is looked up in the `images` folder and it is returned by using the `dd` command.

There is a simple caching implemented, where it checks for the cached files. All the files rendered are written both to the stdout and to the cache directory. The name of the file is the hash of the QUERY_STRING used in the request.

Since I am running this script on a Raspberry PI, I am also blinking an LED, when a request is done. The code is commented out in the main function.

## Tests

Currently this blog is up and running as my personal blog engine. It serves well, wkthout any issues. It is paired with my CGI server, and I didn't have the capacity to test it with others servers like Apache or Lighttpd yet. But it should be running just fine.

## Known issues and todo

The script is very basic, and could be improved. It needs a bit of refactoring to make it more readable.

It also provides only basic sanity check on input.

Caching has no update, meaning it will return the version of the given file used, when creating the cached version. The cache must be manually cleared when updating an existing entry.

Last but not least I am currently using absolute paths for the mustache templates and didn't checked whether it works with relative paths. Be aware, if you try to run this blog.
 
