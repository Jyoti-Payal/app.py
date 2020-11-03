# app.py
from flask import Flask, render_template,request,redirect
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///posts.db'
db = SQLAlchemy(app)


class BlogPost(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    author = db.Column(db.String(20), nullable=False, default='N/A')
    date_posted = db.Column(db.DateTime, nullable=False, default=datetime.utcnow)

    def __repr__(self):
        return 'Blog post' + str(self.id)


all_posts = [
    {
        'title': 'Post 1',
        'content': 'This is the content of post 1. Jyoti is a beautiful girl',
        'author': 'Jyoti'
    },
    {

        'title': 'Post 2',
        'content': 'This is the content of post 2. Jyoti is a beautiful girl'
    }


]


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/posts', methods=['GET', 'POST'])
def posts():
    return render_template('posts.html', posts=all_posts)

@app.route('/home/users/<string:name>/posts/<int:id>')
def hello(name, id):
    return "Hello, " + name + "your id is: "+str(id)


@app.route('/only get', methods=['GET'])
def get_req():
    return 'You can only get this webpage 2.'


if __name__ == "__main__":
    app.run(debug=True)

