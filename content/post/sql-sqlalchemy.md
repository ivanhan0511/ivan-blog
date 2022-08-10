---
title: "Sql Sqlalchemy"
date: 2022-08-10T18:32:18+08:00
categories:
- SQL
- SQLAlchemy
tags:
- SQLAlchemy
- Flask
keywords:
- flask
- sqlalchemy
- relationship
---

I need SQLAlchemy, so far...

<!--more-->

{{< toc >}}

## PRICIPLES

SQL databases behave less like object collections the more size and performance start to matter;  
object collections behave less like tables and rows the more abstraction starts to matter.  
SQLAlchemy aims to accommodate both of these principles.




## DB SESSION


## CRUD TIPS

### Multiple Order by

{{< codeblock "xxx_helper.py" "python" "https://ivanhan0511.github.io" "order by" >}}
def hello():
    print('hello')
{{< /codeblock >}}




### Improvement

#### Relationship

As example:
{{< tabbed-codeblock "lazy relationship" >}}
<!-- tab Model -->
from datetime import datetime


class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(80), nullable=False)
    body = db.Column(db.Text, nullable=False)
    pub_date = db.Column(db.DateTime, nullable=False,
        default=datetime.utcnow)

    category_id = db.Column(db.Integer, db.ForeignKey('category.id'),
        nullable=False)
    category = db.relationship('Category',
        backref=db.backref('posts', lazy=True))

    def __repr__(self):
        return '<Post %r>' % self.title


class Category(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)

    def __repr__(self):
        return '<Category %r>' % self.name
<!-- endtab -->

<!-- tab app -->
>>> py = Category(name='Python')
>>> Post(title='Hello Python!', body='Python is pretty cool', category=py)
>>> p = Post(title='Snakes', body='Ssssssss')
>>> py.posts.append(p)
>>> db.session.add(py)
<!-- endtab -->
{{< /tabbed-codeblock >}}

Let’s look at the posts. Accessing them will load them from the database since the relationship is lazy-loaded, 
    but you will probably not notice the difference - loading a list is quite fast:


>>> py.posts
[<Post 'Hello Python!'>, <Post 'Snakes'>]


## ORM



