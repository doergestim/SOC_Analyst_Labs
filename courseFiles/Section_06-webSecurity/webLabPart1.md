# For the Windows VM

## The objective for this lab is to
- Understand how SQL Injection works
- See how it appears in logs
- See the mitigation ( how a SOC analyst should react )
- See the difference between vulnerable and secure code

## Setup
- Open a terminal



- cd to Desktop and make the lab folder
```bash
cd ~/Desktop
```
```bash
mkdir WebLab
```
```bash
cd WebLab
```
- Create the main app
```bash
sudo nano app.py
```
- Copy paste this code
```bash
from flask import Flask, request, render_template_string, g
import sqlite3, os, datetime

app = Flask(__name__)
DB_PATH = os.path.join(os.path.dirname(__file__), 'lab.db')
LOG_PATH = os.path.join(os.path.dirname(__file__), 'access.log')

TEMPLATE = '''
<!DOCTYPE html>
<html lang="en"><head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width, initial-scale=1">
<title>Vuln Login (Flask)</title>
<style>
body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; background:#0b1220; color:#e5e7eb; display:flex; align-items:center; justify-content:center; height:100vh; margin:0;}
.card { background:#111827; padding:24px; border-radius:16px; width:340px; box-shadow:0 10px 30px rgba(0,0,0,.4)}
h1 { font-size:20px; margin:0 0 12px 0; }
label { font-size:12px; color:#93c5fd; display:block; margin-top:10px;}
input { width:100%; padding:10px; border-radius:8px; border:1px solid #374151; background:#0b1220; color:#e5e7eb; }
button { margin-top:12px; width:100%; padding:10px; border-radius:10px; border:0; background:#2563eb; color:white; font-weight:600; cursor:pointer;}
.hint { font-size:12px; color:#9ca3af; margin-top:8px;}
a { color:#60a5fa; text-decoration:none;}
a:hover { text-decoration:underline;}
</style>
</head>
<body>
  <div class="card">
    <h1>Login ({{mode}})</h1>
    <form method="POST" action="/login/{{mode}}">
      <label>Username</label>
      <input name="username" autocomplete="off" />
      <label>Password</label>
      <input name="password" type="password" />
      <button type="submit">Sign in</button>
    </form>
    <p class="hint">Try normal creds, then <code>' OR '1'='1' -- </code></p>
    <div class="links">
      <a href="/init">Init/Reset DB</a> ·
      <a href="/">Vulnerable</a> ·
      <a href="/secure">Secure</a> ·
      <a href="/logs">View logs</a>
    </div>
    {% if message %}
      <p>{{message|safe}}</p>
    {% endif %}
  </div>
</body>
</html>
'''

def log_line(msg):
    with open(LOG_PATH, 'a', encoding='utf-8') as f:
        f.write(f"[{datetime.datetime.utcnow().isoformat()}Z] {msg}\n")

def get_db():
    conn = sqlite3.connect(DB_PATH)
    conn.row_factory = sqlite3.Row
    return conn

def init_db():
    conn = get_db()
    cur = conn.cursor()
    cur.execute("DROP TABLE IF EXISTS users")
    cur.execute("CREATE TABLE users (id INTEGER PRIMARY KEY AUTOINCREMENT, username TEXT UNIQUE, password TEXT)")
    cur.executemany("INSERT INTO users (username, password) VALUES (?,?)",
                    [('admin','admin123'),('alice','passw0rd'),('bob','hunter2')])
    conn.commit()
    conn.close()

@app.route('/init')
def do_init():
    init_db()
    return 'Database initialized. Users: admin/admin123, alice/passw0rd, bob/hunter2'

@app.route('/')
def index():
    return render_template_string(TEMPLATE, mode='VULN', message='')

@app.route('/secure')
def secure_index():
    return render_template_string(TEMPLATE, mode='SECURE', message='')

@app.route('/login/VULN', methods=['POST'])
def login_vuln():
    username = request.form.get('username','')
    password = request.form.get('password','')
    # Intentionally vulnerable string concatenation
    query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}';"
    log_line(f"VULN login attempt | user={username} | query={query} | ip={request.remote_addr}")
    conn = get_db()
    try:
        rows = conn.execute(query).fetchall()
    except Exception as e:
        rows = []
        log_line(f"SQLite error: {e}")
    conn.close()
    if rows:
        msg = f"<b>Welcome, {username} (UNSAFE)</b><br>Query:<br><code>{query}</code>"
        return render_template_string(TEMPLATE, mode='VULN', message=msg)
    return render_template_string(TEMPLATE, mode='VULN', message=f"Login failed.<br><code>{query}</code>")

@app.route('/login/SECURE', methods=['POST'])
def login_secure():
    username = request.form.get('username','')
    password = request.form.get('password','')
    conn = get_db()
    rows = conn.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password)).fetchall()
    conn.close()
    log_line(f"SECURE login attempt | user={username} | ip={request.remote_addr}")
    if rows:
        msg = f"<b>Welcome, {username} (SECURE)</b><br>Parameterized query used."
        return render_template_string(TEMPLATE, mode='SECURE', message=msg)
    return render_template_string(TEMPLATE, mode='SECURE', message="Login failed (SECURE).")

@app.route('/logs')
def logs():
    if os.path.exists(LOG_PATH):
        with open(LOG_PATH, 'r', encoding='utf-8') as f:
            return "<pre>" + f.read().replace("<","&lt;") + "</pre>"
    return "No logs yet."

if __name__ == '__main__':
    if not os.path.exists(DB_PATH):
        init_db()
    app.run(host='0.0.0.0', port=8000, debug=True)
```
- Do `Ctrl+x` + `y` + `Enter`
- Create requirements
```bash
sudo nano requirements.txt
```
- Copy paste this code
```bash
flask
sqlite3-binary; platform_system=='Windows'
```
- Do `Ctrl+x` + `y` + `Enter`
- Create the virtual environment and install the requirements
```bash
sudo apt update
```
```bash
sudo apt install python3-venv
```

---

- In case that doesn't work do this then continue

```bash
curl -fsSL https://archive.kali.org/archive-key.asc \
| sudo gpg --dearmor -o /usr/share/keyrings/kali-archive-keyring.gpg -y
```

---

```bash
python3 -m venv .venv
```
```bash
source .venv/bin/activate
```
```bash
pip install -r requirements.txt
```
```bash
python app.py
```

<img width="660" height="288" alt="image" src="https://github.com/user-attachments/assets/5cd1297e-f75d-4124-b7b4-86bb1ca403bf" />

To connect to the site open ``http://10.10.119.212:8000`` **NOTE THAT YOUR IP MAY BE DIFFERENT**

## Go to [Part 2](/courseFiles/Section_06-webSecurity/webLabPart2.md)

---
[Back to the section](/courseFiles/Section_06-webSecurity/webSecurity.md)
