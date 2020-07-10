# 1. [M2-6] Dialogue Generator

## 2. Package summery 

The dialogue generator is an agent that generates the robot utterance in human-robot interaction. When the user utterance and user information taken as input values, the robot utterance is generated using the given information. The user information is obtained from the Task Manager through ROS protocol. After the robot utterance created, the generated robot utterance is forwarded to the Task Manager again through the ROS protocol.

- 2.1 Maintainer status: maintained
- 2.2 Maintainer: Yuri Kim, [yurikim@hanyang.ac.kr]()
- 2.3 Author: Yuri Kim, [yurikim@hanyang.ac.kr]()
- 2.4 License (optional): 
- 2.5 Source git: https://github.com/DeepTaskHY/DM_Generator

## 3. Overview

When the user utterance and user information taken as input values, the robot utterance is generated using the given information. The user information is obtained from the Task Manager through ROS protocol. After the robot utterance created, the generated robot utterance is forwarded to the Task Manager again through the ROS protocol.

(여기 다시 써야할 듯 / 구조도 추가하기)

## 4. Hardware requirements

None

## 5. Quick start 

### 5.1 Install dependency:

**Flask**

installation

```
$ pip install flask
```

test code  

```
from flask import Flask  
app = Flask(__name__)  

@app.route('/') 
def hello_world():
	return 'Hello world!'  
	if __name__=='__main__':
		app.run() 
```

**dialogflow api**  

    $ pip install dialogflow  
    $ pip install --upgrade google-auth-oauthlib  
    $ pip install --upgrade pyasn1-modules  

**ngrok**  
Download ngrok from this site('https://ngrok.com/') and run the following command line.  

    $ ./ngrok <portnum>

**ros-kinetic**

    $ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'  
    $ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654  
    $ sudo apt-get update  
    $ sudo apt-get install ros-kinetic-desktop-full  
    $ sudo rosdep init  
    $ rosdep update  

### 5.2 Start the module

- Download ngrok from this site('https://ngrok.com/') and run the following command line. 
  `$  ./ngrok <portnum>  `

- If you have developer permission, in the dialogflow project, add the 'forwarding link (add '/webhook' after the forwarding link)' that appears when you run ngrok to the fulfillment in the dialogflow console. 
  You can test this module in the dialogflow console after running 'social_fulfillment.py'.  

- If you don't have permission, go to Github and download the JSON file(auth key). 
  Add the path of the downloaded JSON file to line 6 of 'dialogflow_client.py'. 
  You can test this module by running the 'dialogflow_client.py' after running 'social_fulfillment.py'  

## 6. Input/Subscribed Topics

```
{
    'header': {
        'timestamp': '1563980674.262554407', 
        'target': ['dialog'], 
        'content': ['dialog_generation'], 
        'source': 'planning'
    }, 
        'dialog_generation': {
        'name': '이병현', 
        'intent': 'transmit_information_health_advice', 
        'id': 177, 
        'human_speech':'응 먹었지.', 
        'social_context': {'take_medicine_schedule': '식후 30분 후'}
    }
}
```

○ header (header/taskExecution): contain information about published time, publisher name, receiver name. 

- timestamp: published time 
- target: receive module name 
- content: role of this ROS topic name 
- source: publish the module name 

○ dialog_generation (dialog_generation/taskExecution): contain required information to generate robot utterance sentence. (Import from Knowledge Manager) 

- intent: sub-task name 
- id: id 
- name: user name 
- gender: user gender 
- appellation: This is the name that the robot uses when calling the user. It is determined by the age of the user. 
- visit_history: Whether the user has visited the hospital. 
- medical_record: User's disease information 
- social_context: required information to generate a robot utterance sentence. (Import from database) 
- take_medicine_schedule: User's medication time 

## 7. Output/Published Topics

```
{
    "header": {
        "timestamp": "%i.%i" % (current_time.secs, current_time.nsecs),
        "source": "dialog",
        "target": ["planning"],
        "content": ["dialog_generation"]
    },
    "dialog_generation": {
        "id": id,
        "dialog": dialog,
        "result": "completion"
    }
}
```

○ header (header/taskExecution): contain information about published time, publisher name, receiver name.  

- timestamp: published time  
- source: publish module name  
- target: receive module name  
- content: role of this ROS topic name  

○ dialog_generation (dialog_generation/taskExecution): contain generated robot speech sentence, id, result.  

- id: id  
- dialog: generated robot speech  
- result: task progress result  

## 8. Parameters

: There are two categories of parameters that can be used to configure the Dialog Generator module: dialogflow client, fulfillment.  

### 8.1 dialogflow client parameters

-  ~GOOGLE_APPLICATION_CREDENTIALS (string, default: None): The path where the authentication key(JSON file) is stored.  
-  ~project_id (string, default: ‘socialrobot-hyu-xdtlug’): Project id of Dialogflow.  
-  ~session_id (string, default: None): It can be a random number or some type of user identifier(preferably hashed). The length of the session ID must not exceed 36 bytes.  
-  ~texts (string, default: None): User’s speech sentence.  
-  ~language_code (string, default: ko): The language of this conversational query. (ko: Korean, en: English)  

### 8.2 fulfillment parameters 

- ~IP_address (string, default:“localhost”): IP address  
- ~port_num (string, default:5000): port number  

## 9. Related Applications (Optional)

None

## 10. Related Publications (Optional)

-  Dialogflow API official document[Website]. (2019, Aug 22). https://cloud.google.com/dialogflow/docs/reference/rest/v2/projects.agent.sessions/detectIntent  
-  ROS Kinetic installation instructions[Website]. (2019, Aug 22). http://wiki.ros.org/kinetic/Installation  
-  Flask user’s Guide[Website]. (2019, Aug 22). https://flask.palletsprojects.com/en/1.1.x/  
-  ngrok[Website]. (2019, Aug 22). https://ngrok.com  

