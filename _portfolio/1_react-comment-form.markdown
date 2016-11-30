---
layout: post
title: Customized React Comment Form
description: with browserify and material design
img:
---

See the Github repo of the finished project: <a href="https://github.com/kmabrahamson/react-tutorial">Customized React Comment Form</a>
<a href="https://github.com/kmabrahamson/react-tutorial"><i class="fa fa-github-square"></i></a>

It finally happened. Enough of my students started coming to me with React questions or expressing their interest in  learning React that it seemed time to figure out what was going on with the new Javascript hotness. The last time I learned a JS framework, AngularJS 1.x, it had been in production and with the support of fellow devs to let me know what was worth focusing on, but this time I was plunging in solo with a series of tutorials and whatever the documentation said to guide me.

I figured I'd get my feet wet with the standard Facebook tutorial, a comment form (though I see that now it's a tic tac toe game). Between the well-written docs and some outside reading, I had a pretty good grasp on what was going on by the time I got to the finished product:

<div class="img_row">
	<img class="col three img-contain" src="{{ site.baseurl }}/img/react-tutorial-v1.png" alt="react-tutorial-v1" title="Sad React Version"/>
</div>
<div class="col three caption">
	I'm imagining a *womp womp* noise right now.
</div>

So, this was the end result after my whirlwind walkthrough of Facebook's React tutorial. It was entirely functional, added new comments, updated content in real time, but I figured I could find a library that would help me make it look a bit more appealing. Design doesn't come intuitively to me, so something like a "Bootstrap for React" that would make all the decisions sounded great. Plus, more practice working with JSX and React components!

	<html>
	  <head>
	    <meta charset="utf-8">
	    <title>React Tutorial</title>
	    <!-- Not present in the tutorial. Just for basic styling. -->
	    <link rel="stylesheet" href="css/base.css" />
	    <script src="https://unpkg.com/react@15.3.0/dist/react.js"></script>
	    <script src="https://unpkg.com/react-dom@15.3.0/dist/react-dom.js"></script>
	    <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
	    <script src="https://unpkg.com/jquery@3.1.0/dist/jquery.min.js"></script>
	    <script src="https://unpkg.com/remarkable@1.7.1/dist/remarkable.min.js"></script>
	  </head>

The other obvious improvement I set my sights on was all the CDN's powering my toy app. Coming from a Rails background, a porky head tag makes me feel like I'm not comfortable with my tools or development environment. I really, really wanted to yank out those CDN's and install those dependencies properly.

I already had npm and NodeJS globally installed on my machine via homebrew, so that left me at picking a bundler.

I chose browserify for two reasons: I'd already used it for another project and it had documentation I could read. Remember this decision: it becomes important later.

I set up a bundle.js file, and then with a few `npm install package --save` commands, I had a package.json that looked less like a scaffold and more like a reasonable app. For one: it had React as a dependency. I also created some npm scripts to start the server and bundle my dependencies for me. So far so good.

	<html>
	  <head>
	    <meta charset="utf-8">
	    <title>React Tutorial</title>
	    <!-- Not present in the tutorial. Just for basic styling. -->
	    <link rel="stylesheet" href="css/base.css" />
	    <script src="https://unpkg.com/jquery@3.1.0/dist/jquery.min.js"></script>
	    <script src="https://unpkg.com/remarkable@1.7.1/dist/remarkable.min.js"></script>
	  </head>
<div class="col three caption">
	So much better, I'm actually sighing in relief looking at it.
</div>

Here's where it got weird.

For my styling library, I settled on <a href="https://www.muicss.com/">MUI</a>, a material design CSS library. Super light weight, just enough to whack my toy app with the pretty stick and then move on. Installed, checked my package.json, but I couldn't for the life of me get its various components and styles to register as available in the application.

Spent a little while banging my head against that one, tried some clever requires straight from the node_modules directory, etc., before I figured out that bundling/transforming CSS isn't in browserify's bag of tricks. Turns out there's browserify-css for that.

Webpack can handle CSS. It has new, approachable documentation now. Didn't a few months ago. Would've been nice.

Another installation, some tweaks of my browserify script, some FontAwesome for fun, and <em>finally</em>...

<div class="img_row">
	<img class="col three img-contain" src="{{ site.baseurl }}/img/react-tutorial-screenshot.png" alt="react-tutorial-finished" title="React, Women-in-Tech style"/>
</div>
<div class="col three caption">
	React, Women-in-Tech style. Bundling CSS makes me punchy.
</div>

...something that looked a little more exciting to interact with. After importing the needed styles, the markup in my JSX render functions remained nicely legible (no inline styling!).

	import React from 'react';
	import ReactDOM from 'react-dom';
	import Styles from '../css/base.css';
	import Container from 'muicss/lib/react/container';
	import Panel from 'muicss/lib/react/panel';
	import Button from 'muicss/lib/react/button';
	import Form from 'muicss/lib/react/form';
	import Input from 'muicss/lib/react/input';

	var CommentBox = React.createClass({
	...
	render: function() {
	  return (
	    <Container>
	      <div className="commentBox">
	        <div className="mui--text-accent mui--text-display4">Comments</div>
	        <CommentList data={this.state.data} />
	        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
	      </div>
	    </Container>
	    );
	  }
	});

I had a lot of fun experimenting with Facebook's offering to the frontend wars, even the toolchain bits and bobs, and I'll probably circle back to Webpack some day soon. I love the idea of component-based development, and I'm excited about React's possibilities as a compliment to Rails 5 with its new API mode. I'm in the process of coming up with more excuses to churn out React sandbox apps, given how old this example is. Electron + React app, maybe?

Source code here: <a href="https://github.com/kmabrahamson/react-tutorial">Customized React Comment Form</a>
<a href="https://github.com/kmabrahamson/react-tutorial"><i class="fa fa-github-square"></i></a>
