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

