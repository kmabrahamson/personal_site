---
layout: post
title: React-Rails LibraryThing Widget
description: fetch API, ES6, and Rails
img: /img/LT-clone-code.png
---

See the Github repo of the finished project: <a href="https://github.com/kmabrahamson/librarything_clone" target="_blank">React-Rails LibraryThing Widget</a>
<a href="https://github.com/kmabrahamson/librarything_clone" target="_blank"><i class="fa fa-github-square"></i></a>

The next step after modifying a few tutorials and following Tyler McGinnis' excellent [ReactJS Fundamentals](https://reacttraining.com/online/react-fundamentals) course was building something of my own, right?

Ever the avid book collector, a lot of my personal projects tend to come back to ways to martial, organize, and play with the metadata in my books and comics. When I realized that [LibraryThing](https://www.librarything.com/), one of my favorite services, had a list of open-source API's to pull from that would let me query the titles in my personal collection, I was incredibly excited to put together a widget that would talk to them.

And thus was born my Rails 5/React-Rails LibraryThing widget. Featuring...

A simple GET request to one of [LibraryThing's API's](https://www.librarything.com/services/), using the [httparty](https://github.com/jnunemaker/httparty) gem. Setting this up as an internal route also allowed me to avoid any CORS header issues.

{% highlight ruby %}
  def books_data
    url = 'https://www.librarything.com/my-librarything-account'
    response = HTTParty.get(url)
    response.parsed_response
    @books = response.parsed_response
    render json: @books.to_json
  end
{% endhighlight %}

I did run into some trouble once this request had successfully been made and passed to my React component (React installed with the [react-rails](https://github.com/reactjs/react-rails) gem). I'm not sure how I expected the returned JSON to be formatted, but the list of nested objects I got back -- after cleaning up the data a bit -- proved to be a challenge to iterate over:

<div class="img_row">
  <img class="col two" src="/img/LT-widget-screenshot1.png" />
  <img class="col one" src="/img/LT-widget-screenshot2.png" />
</div>

<div class="col two caption">
  Oof.
</div>

<div class="col one caption">
  There's all the information I actually want.
</div>

In the browser, I envisioned a dynamically-created table populated by my books' metadata: title, author, publication date, etc. My solution was to use a combination of Object.keys, looping, and pushing the needed tags into an array along the way before passing the final result to the component's return statement.

```jsx
render() {
var bookNames = Object.keys(this.state.data.books)
var obj = ''
var rows = []
// iterate through nested book objects and their attrs
for (let i of bookNames) {
  obj = this.state.data.books[i];
  rows.push(<tr key={i}>
    <td>{obj['author_lf']}</td>
    <td>{obj['title']}</td>
    <td>{obj['publicationdate']}</td>
  </tr>)
}
return (
  <table className="table table-striped">
    <thead>
      <tr className="info">
        <th>Author</th>
        <th>Title</th>
        <th>Publication Date</th>
      </tr>
    </thead>
    <tbody>
      {rows}
    </tbody>
  </table>
)
```

Right now, I have a simple widget that loads twenty random books' information from my collection, and if you click a button it'll grab twenty different ones (LibraryThing's simplest API was built for those WordPress blog widgets and seems to provide only 20 books' information at a time). Next, I'd like to create a side-panel that allows a user to search for written works and see all their returned results, courtesy of a different LibraryThing API. And also some kind of spinner/loading screen for the table; the initial load is a little slow.

Head on over to check it out live!
