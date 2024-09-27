# AccuKnox_QA_Trainee_Role_Problem-2

# System Health Monitoring Script.py

    import psutil
    import logging
    from datetime import datetime

# Set up logging
    
    logging.basicConfig(filename='system_health.log', level=logging.INFO, format='%(asctime)s - %(message)s')

# Thresholds

    CPU_THRESHOLD = 80  # in percentage
    MEMORY_THRESHOLD = 80  # in percentage
    DISK_THRESHOLD = 80  # in percentage
    PROCESS_THRESHOLD = 200  # number of processes
    
    def log_alert(message):
        logging.info(message)
        print(message)
    
    def check_cpu_usage():
        cpu_usage = psutil.cpu_percent(interval=1)
        if cpu_usage > CPU_THRESHOLD:
            log_alert(f"High CPU usage detected: {cpu_usage}%")
    
    def check_memory_usage():
        memory = psutil.virtual_memory()
        memory_usage = memory.percent
        if memory_usage > MEMORY_THRESHOLD:
            log_alert(f"High Memory usage detected: {memory_usage}%")
    
    def check_disk_usage():
        disk = psutil.disk_usage('/')
        disk_usage = disk.percent
        if disk_usage > DISK_THRESHOLD:
            log_alert(f"High Disk usage detected: {disk_usage}%")
    
    def check_running_processes():
        processes = len(psutil.pids())
        if processes > PROCESS_THRESHOLD:
            log_alert(f"High number of running processes detected: {processes}")
    
    if _name_ == "_main_":
        check_cpu_usage()
        check_memory_usage()
        check_disk_usage()
        check_running_processes()

# AppHealth.py

    import requests
    import logging
    
    # Set up logging
    logging.basicConfig(filename='app_health.log', level=logging.INFO, format='%(asctime)s - %(message)s')
    
    # Application URL
    APP_URL = 'http://your_application_url_here'
    
    def check_application_health():
        try:
            response = requests.get(APP_URL)
            if response.status_code == 200:
                message = "Application is up and running."
            else:
                message = f"Application is down. Status code: {response.status_code}"
            logging.info(message)
            print(message)
        except requests.exceptions.RequestException as e:
            message = f"Application is down. Error: {e}"
            logging.error(message)
            print(message)
    
    if _name_ == "_main_":
        check_application_health()

# SystemHealth.py

    from flask import Flask, jsonify, render_template
    import psutil
    import logging
    import os

# Initialize Flask application

    app = Flask(_name_)

# Set up logging

    logging.basicConfig(filename='app_health.log', level=logging.INFO, format='%(asctime)s - %(message)s')

# Thresholds for system health checks

    CPU_THRESHOLD = 80  # in percentage
    MEMORY_THRESHOLD = 80  # in percentage
    DISK_THRESHOLD = 80  # in percentage
    PROCESS_THRESHOLD = 200  # number of processes

# Path to local application file to check
    APP_FILE_PATH = 'D:/B.TECH/Mini Project/QA/por/po/index.html'  # Replace with your actual file path
    
    def log_alert(message):
        logging.info(message)
        print(message)
    
    def check_cpu_usage():
        cpu_usage = psutil.cpu_percent(interval=1)
        if cpu_usage > CPU_THRESHOLD:
            log_alert(f"High CPU usage detected: {cpu_usage}%")
    
    def check_memory_usage():
        memory = psutil.virtual_memory()
        memory_usage = memory.percent
        if memory_usage > MEMORY_THRESHOLD:
            log_alert(f"High Memory usage detected: {memory_usage}%")
    
    def check_disk_usage():
        disk = psutil.disk_usage('/')
        disk_usage = disk.percent
        if disk_usage > DISK_THRESHOLD:
            log_alert(f"High Disk usage detected: {disk_usage}%")
    
    def check_running_processes():
        processes = len(psutil.pids())
        if processes > PROCESS_THRESHOLD:
            log_alert(f"High number of running processes detected: {processes}")
    
    def check_application_health():
        if os.path.exists(APP_FILE_PATH):
            message = "Application is up and running."
            logging.info(message)
            print(message)
        else:
            message = "Application file not found."
            logging.error(message)
            print(message)
    
    @app.route('/')
    def index():
        return render_template('index.html')
    
    @app.route('/api/system_health', methods=['GET'])
    def get_system_health():
        check_cpu_usage()
        check_memory_usage()
        check_disk_usage()
        check_running_processes()
     
    return jsonify(message="System health checks completed."), 200

    @app.route('/api/application_health', methods=['GET'])
    def get_application_health():
        check_application_health()
    
    return jsonify(message="Application health check completed."), 200

    if _name_ == '_main_':
        app.run(debug=True)

# Health.py

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Application Health Checker</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                padding: 20px;
            }
            .status {
                padding: 10px;
                border-radius: 5px;
                margin-top: 20px;
                display: inline-block;
            }
            .up {
                background-color: #d4edda;
                color: #155724;
            }
            .down {
                background-color: #f8d7da;
                color: #721c24;
            }
        </style>
    </head>
    <body>
        <h1>Application Health Checker</h1>
        <button onclick="checkHealth()">Check Application Health</button>
        <div id="status" class="status"></div>

    <script>
        function checkHealth() {
            fetch('/api/health')
                .then(response => response.json())
                .then(data => {
                    const statusDiv = document.getElementById('status');
                    if (data.status === 'up') {
                        statusDiv.innerText = 'Application is up and running.';
                        statusDiv.className = 'status up';
                    } else {
                        statusDiv.innerText = Application is down. Status code: ${data.status_code || ''} ${data.error || ''};
                        statusDiv.className = 'status down';
                    }
                })
                .catch(error => {
                    const statusDiv = document.getElementById('status');
                    statusDiv.innerText = Error: ${error};
                    statusDiv.className = 'status down';
                });
        }
    </script>
    </body>`
    </html>

# Health.js

    etch('/api/index.html')

    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        const statusDiv = document.getElementById('status');
        if (data.message && data.message.includes('up')) {
            statusDiv.innerText = 'Application is up and running.';
            statusDiv.className = 'status up';
        } else {
            statusDiv.innerText = Application is down. ${data.message || ''};
            statusDiv.className = 'status down';
        }
    })
    .catch(error => {
        const statusDiv = document.getElementById('status');
        statusDiv.innerText = Error: ${error};
        statusDiv.className = 'status down';});

# Health.json

    {
    "message": "Application is up and running."
    }
