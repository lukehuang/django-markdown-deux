A small Django app that provides template tags for using
[Markdown](http://daringfireball.net/projects/markdown/) using the
[python-markdown2](http://code.google.com/p/python-markdown2/) library.

# What's with the "deux" in the name?

The obvious name for this project in `django-markdown2`. However, there
[already is one!](http://github.com/svetlyak40wt/django-markdown2) and name
confusion doesn't help anybody. Plus, I took French immersion in school for 12
years: might as well put it to use.

# So why another project then?

Because I wanted to do something slightly different. Django-markdown2's
`markdown` filter takes
["extras"](http://code.google.com/p/python-markdown2/wiki/Extras) as arguments
-- with the one exception that "safe" is transformed to python-markdown2's
"safe_mode" argument. This is handy for quick usage. My use case is more
commonly: lots of `markdown` filter and block usage in my Django templates with
the same set of python-markdown2 options.


# Installation

Choose the *one* of the following that works best for you:

- Get a git clone of the source tree:

        git clone git://github.com/trentm/django-markdown-deux.git

    You might want a particular tag:

        cd django-markdown-deux
        git tag -l   # list available tags
        git checkout $tagname

    Then you'll need the "lib" subdir on your PYTHONPATH:

        python setup.py install # or 'export PYTHONPATH=`pwd`/lib:$PYTHONPATH'

TODO: pip/pypm instructions when have this up on pypi


Note that django-markdown-deux requires the [python-markdown2
library](http://code.google.com/p/python-markdown2]:

    svn ls http://python-markdown2.googlecode.com/svn/tags
    svn co http://python-markdown2.googlecode.com/svn/tags/$tagname python-markdown2
    cd python-markdown2
    python setup.py install   # or 'export PYTHONPATH=`pwd`/python-markdown2/lib'

    # or

    pip install markdown2     # or with ActivePython: 'pypm install markdown2'


# Django project setup

1. Add "markdown_deux" to `INSTALLED_APPS` in your project's "settings.py".

2. Optionally set some of the `MARKDOWN_DUEX_*` settings. See the [Settings
   section](#settings) below.


# Usage

The markdown_deux facilities typically take an option "style" argument. This
is a name for a set of options to the `python-markdown2` processor. See the
`MARKDOWN_DEUX_STYLES` setting below for more.

## `markdown` template filter

    {% load markdown_deux_tags %}
    ...
    {{ myvar|markdown:"STYLE" }}      {# convert `myvar` to HTML using the "STYLE" style #}
    {{ myvar|markdown }}              {# same as `{{ myvar|markdown:"default"}}` #}

## `markdown` template block tag

    {% load markdown_deux_tags %}
    ...
    {% markdown STYLE %}        {# can omit "STYLE" to use the "default" style #}
    This is some **cool**
    [Markdown](http://daringfireball.net/projects/markdown/)
    text here.
    {% endmarkdown %]

## `markdown_allowed` template tag

In a template:

    {% markdown_allowed %}

will emit a short HTML blurb that says Markdown syntax is allowed. This can be
handy for placing under form elements that accept markdown syntax. You can also
use it as the `help_text` for a form field something like:

    # myapp/forms.py
    from markdown_deux.templatetags.markdown_deux_tags import markdown_allowed
    class MyForm(forms.Form):
        #...
        description = forms.CharField(
            label="Description (required)",
            widget=forms.Textarea(attrs={"rows": 5}),
            help_text=_secondary_span("A brief description of your thing.<br/> "
                markdown_allowed()),
            required=True)


## `markdown_cheatsheet` tag

    {% markdown_cheatsheet %}

This outputs HTML giving a narrow (appropriate for, e.g., a sidebar) listing of
some of the more common Markdown features.


## `markdown_deux.markdown(TEXT, STYLE)` in your Python code

The `markdown` filter and block tags above ultimately use this
`markdown_deux.markdown(...)` function. You might find it useful to do Markdown
processing in your Python code (e.g. in a view, in a model `.save()` method).


# Settings

TODO

