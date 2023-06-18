# hr-test-python

This is the interview questions for python dev skills.

---
### 1. How do you setup local python dev environment?  
- What IDE do you use?
  PyCharm
- How do you setup python production environment in Linux?
  - List the cli commands if possible.
    1. Install pip if it is not in your system. It is assumed python is installed\
      $ sudo apt-get install python-pip
    2. Install virtualenv\
      $ pip install virtualenv
    3. Create a virtual environment. After this command, a folder named virtualenv_folder will be created.\
      $ virtualenv virtualenv_folder
    4. If you want to create a virtualenv for a specific Python version, type\
      $ virtualenv -p /usr/bin/python3 virtualenv_folder
    5. Activate it\
      $ source virtualenv_folder/bin/activate
    6. To deactivate\
      $ deactivate
    

---
### 2. Are you familiar using any linux distro?
      I am familiar using ubuntu.
- crontab: Crontab is a command-line utility in Linux operating systems that allows users to schedule and automate recurring tasks or jobs.
  
- ssh: SSH is a cryptographically secure communication method that enables two computers to communicate over an unsecured line. 
  
- nfs:  NFS is a distributed file system protocol.
  
- nginx: Nginx is a popular open-source web server software and reverse proxy server.

  
---
### 3. Setup a RESTful API with python & nginx.
- Using localhost
- Free to choose any python web component
- All outputs in JSON

API definition: /timestamp
> return current UNIX timestamp
```
# Test command:
curl 127.0.0.1/timestamp

# Expected outcome:
{"timestamp":1624900201}
```

API definition: /readdata
> read POST JSON input and send it back
```
# Test command:
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"username":"abc","password":"xyz"}' \
  http://127.0.0.1/readdata

# Expected outcome:
{"username":"abc","password":"xyz"}
```

bad input
```
# Test command:
curl 127.0.0.1/noexist

# Expected outcome:  (404)
... 
404 Not Found 
...
```

#### What to submit:
```
from flask import Flask, jsonify, request
import time

app = Flask(__name__)

@app.route('/timestamp')
def curr_time():
    current_timestamp = int(time.time())
    return jsonify(timestamp=current_timestamp)

@app.route('/readdata', methods=['POST'])
def read_send():
    data = request.get_json()
    return jsonify(data)

@app.errorhandler(404)
def not_found_error(error):
    return jsonify(error='404 Not Found'), 404


if __name__ == '__main__':
    app.run(host='127.0.0.1', port=80)
```
---
### 3.1 Nginx (Optional)
- Use Nginx as the front web server (reverse proxy)
- deploy python program on port 8000

#### What to submit:
- Nginx server config
  ```
  
  server {
    listen 80;
    server_name domain_name;

    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
  }
  
```
