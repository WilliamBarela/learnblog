---
layout: post
title:  Vim Markup Input Tool
date:   2017-06-05 07:29:07 +0000
---


Although Vim is notorious for having a steep learning curve, I have been using it off and on for nearly four years now, so I feel fairly confident making my way around Vim. Neveretheless, when it comes to editing HTML markup and CSS, I have tended to shy away from it in favor of Atom or another text editor which natively supports a large set of autocompletions for the tags of those scripts. However, today that has changed. 

I love Vim and I thoroughly enjoy the workflow of Vim when combined with Gnu-Screen. Having all of the power of the Bash terminal in your editor and not needing to ever touch a mouse has always made Vim my editor of choice. As a result, when I started writing eRuby files for Sinatra, I decided that I needed to find a plugin which was robust enough to be able to let me enjoy those same efficiencies which you can gain in Atom when working with HTML files. What I found was much more.

The Vim plugin **[emmet-vim](https://github.com/mattn/emmet-vim/)**, by Yasuhiro Matsumoto, is a powerful and extensible plugin for Vim which is very similar to Emmet which can be used in editors like Sublime. The following video from the Github repo shows how quickly one can input HTML and CSS by using the logical snippets which follow rules similar to those used in CSS:

![](https://raw.githubusercontent.com/mattn/emmet-vim/master/doc/screenshot.gif)

## How to Install it

If you do not use a package manager in Vim, you can run the following script in Bash to easily install emmet-vim:

```
git clone git@github.com:mattn/emmet-vim.git #if you use SSH for git
# git clone https://github.com/mattn/emmet-vim.git #if you use HTTP for git
cd emmet-vim
mkdir ~/.vim/plugin/
mkdir ~/.vim/autoload/
cp plugin/emmet.vim ~/.vim/plugin/
cp autoload/emmet.vim ~/.vim/autoload/
cp -a autoload/emmet ~/.vim/autoload/
```

## How to Use it
### The Basics

Typing `ctrl + y` then `,` after any tag will expand it

For example:

* `form` becomes `<form action=""></form>`
* `input#my_id` becomes `<input id="my_id" type="">`

### More advanced

You can also join several options in a string before typing `ctrl + y ,`. Doing so will yield lighting fast expansions of HTML tag structures.

The most simplistic example is simply typing bang, `!`. This expands to an HTML5 document:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title></title>
</head>
<body>
  
</body>
</html>
```


But if that seems too magical and rigid, try this. Simply type `div>ul>li#my_li_id${Hello, world!}*5` to get:

```
<div>
  <ul>
    <li id="my_li_id1">Hello, world!</li>
    <li id="my_li_id2">Hello, world!</li>
    <li id="my_li_id3">Hello, world!</li>
    <li id="my_li_id4">Hello, world!</li>
    <li id="my_li_id5">Hello, world!</li>
  </ul>
</div>
```
## Extending the library
If you like what you have just seen, you are going to love what you can do with just few additional lines to your `~/.vimrc`. Emmet-vim supports the addition of your own custom expansions and allows you to specify which languages it should match. Since the erb tags of Sinatra have become a common place occurance in my code, I decided to write my own modifications to create the scripting tag `<% %>`, and the substitutional tag `<%= %>`. And, I also wanted to make one which would make a scripting tag specifically for iterators in Ruby.

It is important to note that one must use `eruby`, not `erb` when creating these settings. To make these modifications, I included the following code in my `~/.vimrc`:

```
 let g:user_emmet_settings = { 
 \  'eruby' : { 
 \    'extends' : 'html',
 \    'snippets' : { 
 \      'erb' : "<% | %>\n\t${child}\n<% end %>",
 \      'e' : "<% | %>\n",
 \      'ee' : "<%= | %>\n",
 \    }   
 \  }
 \}
```
The snippets are the key value pairs of keywords to type and tags which you want them to expand to. Making this simple modification in my `~/.vimrc`, I am now able to write the following:

`form:post>erb>ul>li*3>ee`

which becomes:

```
<form action="" method="post">
  <%  %>  
    <ul>
      <li><%=  %></li>
      <li><%=  %></li>
      <li><%=  %></li>
    </ul>
        
  <% end %>
</form>
```

I hope you find this little discovery as helpful as it was to me. Happy Viming!

# Bonus
If you ever find the need to remove the content inside of an HTML tag, the easiet way is to type `vit`. Doing so, you can choose to `c`hange, or `y`ank the text from within the tags. You can also type `vat` to select the whole tag and all of its contents. Lastly, a cheatseet for commands that you can run in Emmet can be found [here](https://docs.emmet.io/cheat-sheet/)
