1. at the start of the project, I built a python file to follow the CPU and Memory power,with a psutil modle that halps as with the monitoring.
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

2.after that I take a templeate that helps me to create a web more nice...
```shell 
<!DOCTYPE html>
<html>
<head>
    <title>System Monitoring</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <style>
        .plotly-graph-div {
            margin: auto;
            width: 50%;
            background-color: rgba(151, 128, 128, 0.688);
            padding: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>System Monitoring</h1>
        <div id="cpu-gauge"></div>
        <div id="mem-gauge"></div>
        {% if message %}
        <div class="alert alert-danger">{{ message }}</div>
        {% endif %}
    </div>
    <script>
        var cpuGauge = {
            type: "indicator",
            mode: "gauge+number",
            value: {{ cpu_metric }},
            gauge: {
                axis: { range: [null, 100] },
                bar: { color: "#1f77b4" },
                bgcolor: "white",
                borderwidth: 2,
                bordercolor: "#ccc",
                steps: [
                    { range: [0, 50], color: "#d9f0a3" },
                    { range: [50, 85], color: "#ffeb84" },
                    { range: [85, 100], color: "#ff5f5f" }
                ],
                threshold: {
                    line: { color: "red", width: 4 },
                    thickness: 0.75,
                    value: {{ cpu_metric }}
                }
            }
        };
        
        var memGauge = {
            type: "indicator",
            mode: "gauge+number",
            value: {{ mem_metric }},
            gauge: {
                axis: { range: [null, 100] },
                bar: { color: "#1f77b4" },
                bgcolor: "white",
                borderwidth: 2,
                bordercolor: "#ccc",
                steps: [
                    { range: [0, 50], color: "#d9f0a3" },
                    { range: [50, 85], color: "#ffeb84" },
                    { range: [85, 100], color: "#ff5f5f" }
                ],
                threshold: {
                    line: { color: "red", width: 4 },
                    thickness: 0.75,
                    value: {{ mem_metric }}
                }
            }
        };
        
        var cpuGaugeLayout = { title: "CPU Utilization" };
        var memGaugeLayout = { title: "Memory Utilization" };
        
        Plotly.newPlot('cpu-gauge', [cpuGauge], cpuGaugeLayout);
        Plotly.newPlot('mem-gauge', [memGauge], memGaugeLayout);
    </script>
</body>
   ```

