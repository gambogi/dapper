# Dapper

A publishing tool for static websites.

Distributed as a Perl module, `App::Dapper` comes with an application called
`dapper` which you can use from the command line and makes it easy to use
the `App::Dapper` perl module to create static websites.

Dapper has three goals:

1. **Simple**. Learning Dapper is easy -- it gets out of the way so you can write content, develop layouts, and deploy to production the way you want.

2. **Flexible**. Content is written in Markdown, templates are written in Liquid.
Dapper combines the two and creates a static website based on your instructions.

3. **Pragmatic**. The easy things are easy and the hard things are possible.
Dapper was created to solve problems in a straight-forward and intuitive way.

Features of Dapper are summarized as follows:

* Written in perl, available as a command line utility after installing.
* Content (pages, posts) is written in Markdown.
* Layouts are developed using the Liquid templating engine.
* Configuration is done extensively via YAML.

The rest of this document shows you how to get your righeously static site set
up and running, how to create and manage content, how to tweak the way it looks,
and how to deploy to various environments.

## Installation

Get Dapper in seconds:

    $ cpanm App::Dapper

Then, create a new site, build it, and view it locally like so:

    $ mkdir new-site && cd new-site
    $ dapper init
    $ dapper serve
    
After that, browse to [http://localhost:8000](http://localhost:8000) to see your site. To modify the content, look at `_source/index.md`. To modify the layout, edit `_layout/index.html`.

## Usage 

When you install the `App::Dapper` Perl module, an executable named `dapper` will be available to you in your terminal window. You can use this executable in a number of ways:

### Init

Using the `dapper` executable, initialize a new site. Note that the site will be 
initialized in the current directory. Therefore, it's usually best to create a blank
directory that can be used for this purpose:

    # Initialize the current directory with a fresh skeleton of a site
    $ dapper [-solc] init

### Build

After the site has been initialized, build it. The process of building a site in
Dapper is easy. Dapper takes care of reading the source files, combining them with
the layout template(s), and saving the result in the `_output` directory.

    # Build the site
    $ dapper [-solc] build

### Serve

Once the site has been built, the resulting website can be viewed using a built-in
web server to Dapper. This built-in web server should be used for development only.

    # Serve the site locally at http://localhost:8000
    $ dapper [-solc] serve

Note that when you are running `dapper serve`, Dapper will both run the web server
and also watch all of the `_source` and `_layout` files and re-build the site when
any of those files change.

The way the `dapper serve` command was built, it makes it easy to have one terminal
window open that is serving and auto-rebuilding the site. Other terminal windows can be opened to modify, create, or delete other files in the project.

### Watch

Dapper has a commmand available called `watch` that allows the user to watch and
re-build the output files if any of the input files (`_source/*`, `_layout/*`)
change. Because this command is invoked any time the `dapper serve` command is
invoked, it is not necessary in most cases. However, it's documented here for 
completeness.

    # Rebuild the site if anything (source, layout dirs; config file) changes
    $ dapper [-solc] watch

### Help

To get more information about command-line options available, the current version
of Dapper that you have installed on your system, or further documentation
available, see the followig:

    # Get help on usage and switches
    $ dapper -h

    # Print the version
    $ dapper -v

# Tutorial

## Initialize a Project

Dapper helps you build static websites. To get you started, you can use the
`dapper init` command to initialize a directory. After running this command,
the following directory structure will be created:

    _config.yml
    _layout/
        index.html
    _source/
        index.md

Now, let's walk through each file:

**_config.yml**

The configuration file is a YAML file that specifies key configuration
elements for your static website. The default configuration file is as
follows:

    ---
    name : My Site

If you want to use a separate source, layout, or output directory, you may
specify it in this file. For instance:

    ---
    name : My Site
    source : _source
    layout : _layout
    output : _output

All of the configurations in this file are available in layout templates,
based on the Liquid template system. For instance, `name` in the
configuration file may be used in a template as follows:

    {{ site.name }}

**_source/index.md**

A sample markdown file is available in the _source directory. Contents:

    ---
    layout: index
    title: Welcome
    ---

    Hello world.

There are a few things to note about this file:

1. There is a YAML configuration block at the start of the file.

2. The *layout* configuration specifies which layout to use.

3. The `index` layout indicates that `_layout/index.html` should be used.

4. The `title` configuration is the name of the post/page. It is optional.

5. All of the configurations may be used in the corresponding layout file.

    <!-- Example use of "name" in a layout file -->
    {{ page.name }}

**_layout/index.html**

Layout files are processed using the Liquid template system. The initial layout
file that is given after you run the `dapper init` command, is this:

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
    <head>
      <title>{{ page.title }}</title>
      <meta http-equiv="content-type" content="text/html; charset=iso-8859-1">
    </head>

    <body>

    {{ page.content }}

    </body>
    </html>

The main content of the text file that is being rendered with this template
is available using `{{ page.content }}`.

Definitions specified in the `_config.yml` file can be referenced under the
"site" namespace (e.g. {{ site.name }}. Definitions specified in the YAML
portion of text files can be referenced under the "page" namespace (e.g.
{{ page.title }}.

## Build the Project

In that same directory, you may then build the site using the `dapper build`
command, which will combine the source files and the layout files and place
the results in the output directory (default: `_output`). After you build
the default site, you'll then have the following directory structure:

    _config.yml
    _layout/
        index.html
    _source/
        index.md
    _output/
        index.html

Now, you'll see a new directory that has been created (`_output`) that contains a single file (`index.html`) containing a mashup of the source file and the layout template.

**_output/index.html**

The output file that is created is a mix of the input file and the layout that
is specified by the input file. For the default site, the following output
file is created:

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
    <head>
      <title>Welcome</title>
      <meta http-equiv="content-type" content="text/html; charset=iso-8859-1">
    </head>

    <body>

    <p>Hello world.</p>

    </body>
    </html>

## View the Results

To see what your website looks like, run the `dapper serve` command which
will spin up a development webserver and serve the static files located in
the output directory (default: `_output`) at L<http://localhost:8000>.

# References

You can look for more information here:

* [CPAN](http://search.cpan.org/dist/App-Dapper/)

* [Github](https://github.com/markdbenson/dapper)

* [Issues](https://github.com/markdbenson/dapper/issues)

* [Historical Releases on BackPAN](http://backpan.perl.org/authors/id/M/MD/MDB/)

* [Continuous Integration Status](https://travis-ci.org/markdbenson/dapper)

# Appendix A: Install from Source

A list of past releases can be found on
[backpan](http://backpan.perl.org/authors/id/M/MD/MDB/). After downloading
and unpacking the release, do this:

    $ perl Makefile.PL
    $ make
    $ make test
    $ make install

Dapper requires Perl 5.14 or greater. To find which version of dapper you have 
installed, do this:

    $ dapper -v

# Appendix B: Working with the Perl Module Directly

Dapper may be used as a perl module directly from a script. Examples:

    use App::Dapper;

    # Create a Dapper object
    my $d = App::Dapper->new();

    # Initialize a new website in the current directory
    $d->init();

    # Build the site
    $d->build();

    # Serve the site locally at http://localhost:8000
    $d->serve();

After installing Dapper, you can find documentation for this module with the
perldoc command.

    perldoc App::Dapper

# Author

Mark Benson [markbenson@vanilladraft.com](mailto:markbenson@vanilladraft.com)

# License and Copyright

The MIT License (MIT)

Copyright (c) 2002-2014 Mark Benson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

