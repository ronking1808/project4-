1. at the start of the project, I built a python file to follow the CPU and Memory power. 
``` python
import psutil
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    cpu_percent = psutil.cpu_percent()
    mem_percent = psutil.virtual_memory().percent
    message = None

    if cpu_percent > 80 or mem_percent > 80:
        message = "High CPU or High Memory, Please scale up!"

    return f"CPU Utilization: {cpu_percent}% and Memory Utilization: {mem_percent}%\n{message or ''}"

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

4.
