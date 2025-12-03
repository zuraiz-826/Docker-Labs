```bash


#!/bin/bash

cd
ls
mkdir flaskwork
cd flaskwork
pwd

python3 -m venv venv
sleep 1
. venv/bin/activate
which python

pip install --upgrade pip
pip install flask
pip list | tail -5

cat > app.py << 'EOF'
from flask import Flask
app = Flask(__name__)
@app.route('/')
def index():
    return "Hello World"
EOF

export FLASK_APP=app.py
echo $FLASK_APP
ls -la
echo "done"







#!/bin/bash

cd ~
ls
mkdir flask_routes
cd flask_routes
touch app.py
sleep 0.3

cat > app.py << 'EOF'
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    print("Rendering Home Page")
    return 'Welcome to the Home Page!'

@app.route('/about')
def about():
    print("Rendering About Page")
    return 'This is the About Page.'

if __name__ == '__main__':
    app.run(debug=True)
EOF

python3 app.py &
sleep 2
echo "check localhost:5000 and /about"
ps aux | grep python
sleep 1




#!/bin/bash

cd ~
mkdir routeparams
cd routeparams
sleep 0.2

cat > paramapp.py << 'EOF'
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return "Route Params Lab Ready"

@app.route('/user/<username>')
def show_user(username):
    print(f"Accessed user: {username}")
    return f"Hello, {username}! Welcome."

if __name__ == '__main__':
    app.run(debug=True)
EOF

python3 paramapp.py &
PID=$!
echo $PID > /tmp/flaskapp.pid
sleep 2
echo "try: curl localhost:5000/user/Alice"
echo "or:  curl localhost:5000/user/Bob"
sleep 1



#!/bin/bash

cd ~
mkdir flask_templates
cd flask_templates
mkdir templates 2>/dev/null
sleep 0.4

cat > app.py << 'EOF'
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def home():
    msg = "Hello from Flask with Jinja2"
    return render_template('index.html', message=msg)

if __name__ == '__main__':
    app.run(debug=True)
EOF

cd templates
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Flask Lab</title></head>
<body>
<h1>Template Test</h1>
<p>{{ message }}</p>
</body>
</html>
EOF

cd ..
python3 app.py &
sleep 3
echo "open browser to localhost:5000"
ps aux | grep python | grep -v grep



#!/bin/bash

cd ~
mkdir template_inherit
cd template_inherit
mkdir templates
sleep 0.2

cd templates
cat > base.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
<title>{% block title %}Base{% endblock %}</title>
</head>
<body>
<header><h1>Site Header</h1></header>
{% block content %}
<p>Default content</p>
{% endblock %}
<footer>Footer here</footer>
</body>
</html>
EOF

cat > child.html << 'EOF'
{% extends "base.html" %}
{% block title %}Child Page{% endblock %}
{% block content %}
<h2>Child Content</h2>
<p>This is from child template</p>
{% endblock %}
EOF

cd ..
cat > app.py << 'EOF'
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def index():
    return render_template('child.html')

if __name__ == '__main__':
    app.run(debug=True)
EOF

python3 app.py &
sleep 2
echo "check localhost:5000"
curl -s http://localhost:5000 | grep -o "Child Content" || echo "running"


#!/bin/bash

cd ~
mkdir form_lab
cd form_lab
mkdir templates 2>/dev/null
sleep 0.3

cat > app.py << 'EOF'
from flask import Flask, request, render_template_string

app = Flask(__name__)

FORM_HTML = '''
<h2>Submit Info</h2>
<form method="post">
Name: <input type="text" name="name"><br>
Email: <input type="email" name="email"><br>
<input type="submit">
</form>
'''

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        name = request.form.get('name', '')
        email = request.form.get('email', '')
        return f"Got: {name}, {email}"
    return FORM_HTML

if __name__ == '__main__':
    app.run(debug=True)
EOF

python3 app.py &
sleep 2
echo "form at localhost:5000"
curl -s http://localhost:5000 | grep -q "Submit Info" && echo "form up"


#!/bin/bash

cd ~
mkdir request_lab
cd request_lab
sleep 0.2

cat > app.py << 'EOF'
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def root():
    return "Request Data Lab"

@app.route('/query')
def query():
    p1 = request.args.get('param1', 'default')
    p2 = request.args.get('param2', '0')
    return f'param1: {p1}, param2: {p2}'

@app.route('/submit', methods=['POST'])
def submit():
    name = request.form.get('name', '')
    age = request.form.get('age', '')
    return f'Got: {name}, {age}'

if __name__ == '__main__':
    app.run(debug=True)
EOF

cat > form.html << 'EOF'
<form action="/submit" method="post">
Name: <input name="name"><br>
Age: <input name="age"><br>
<input type="submit">
</form>
EOF

python3 app.py &
sleep 2
echo "test: curl 'localhost:5000/query?param1=test'"
echo "form at form.html"
ps | grep python | grep -v grep


#!/bin/bash

cd ~
mkdir urlfor_lab
cd urlfor_lab
sleep 0.3

cat > app.py << 'EOF'
from flask import Flask, redirect, url_for

app = Flask(__name__)

@app.route('/')
def index():
    return "Index page"

@app.route('/home')
def home():
    return "Home page here"

@app.route('/go_home')
def go_home():
    return redirect(url_for('home'))

if __name__ == '__main__':
    app.run(debug=True)
EOF

python3 app.py &
sleep 2
echo "test: curl -L localhost:5000/go_home"
curl -s http://localhost:5000/go_home 2>/dev/null | head -1



#!/bin/bash

cd ~
mkdir flash_lab
cd flash_lab
mkdir templates 2>/dev/null
sleep 0.2

cat > app.py << 'EOF'
from flask import Flask, flash, render_template, request, redirect, url_for

app = Flask(__name__)
app.secret_key = 'labkey123'

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        data = request.form.get('data')
        if data:
            flash('Success! Data received', 'success')
        else:
            flash('Error: no data entered', 'error')
        return redirect(url_for('index'))
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
EOF

cd templates
cat > index.html << 'EOF'
<html>
<body>
<h3>Flash Test</h3>
{% with messages = get_flashed_messages() %}
  {% if messages %}
    <ul>
    {% for msg in messages %}
      <li>{{ msg }}</li>
    {% endfor %}
    </ul>
  {% endif %}
{% endwith %}
<form method="post">
<input name="data" placeholder="enter something">
<button>Submit</button>
</form>
</body>
</html>
EOF

cd ..
python3 app.py &
sleep 3
echo "open browser to localhost:5000"
ps aux | grep python | grep -v grep | head -1



#!/bin/bash

cd ~
mkdir static_lab
cd static_lab
mkdir static
mkdir templates
sleep 0.2

cat > app.py << 'EOF'
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
EOF

cd static
cat > style.css << 'EOF'
body { background: #f5f5f5; font-family: sans-serif; }
h1 { color: #333; }
EOF

cd ../templates
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="/static/style.css">
</head>
<body>
<h1>Static Files Test</h1>
<p>Page with custom CSS</p>
</body>
</html>
EOF

cd ..
python3 app.py &
sleep 2
echo "open localhost:5000"
curl -s http://localhost:5000 | grep -o "Static Files Test"


```

