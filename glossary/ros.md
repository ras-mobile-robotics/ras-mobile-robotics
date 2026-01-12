# ROS 2

In ROS 2, a robot is broken down into many small, specialized programs called **Nodes**. To perform complex tasks, these nodes must talk to each other using four primary communication patterns: **Topics, Services, Actions, and Parameters**.

---

## 1. Nodes
A **Node** is a single process that performs a specific task (e.g., one node controls the wheels, one node reads the LIDAR, and one node plans a path). 

- **Think of it as:** A specialized worker in a factory.
- **Why?** If the "LIDAR Node" crashes, the "Wheel Node" can still stop the robot safely.

---

## 2. Topics (Publish / Subscribe)



Topics are used for **continuous data streams**. A node "Publishes" data to a topic, and any node that "Subscribes" to that topic receives the data.

- **Pattern:** Many-to-Many, Unidirectional.
- **Think of it as:** A radio broadcast. The station (Publisher) sends music out; anyone with a radio (Subscriber) can listen, but they can't talk back to the station through the same frequency.
- **Best for:** Sensor data (LIDAR, Camera, Battery Level).

---

## 3. Services (Client / Server)



Services are used for **quick, "Question and Answer" interactions**. A Client sends a request, and a Server sends a response.

- **Pattern:** **One Server / Many Clients**, Bidirectional (Request/Response), Synchronous.
- **Think of it as:** Ordering food at a counter. You ask for a burger (Request), and the clerk gives you the burger (Response).
- **Best for:** Changing a setting, triggering a one-time calculation, or toggling a light.

---

## 4. Actions (Goal / Feedback / Result)



Actions are for **long-running tasks**. Unlike services, actions provide feedback while the task is happening and can be canceled mid-way.

- **Pattern:** Goal-oriented with continuous feedback.
- **Think of it as:** Ordering an Uber. You set a destination (Goal). The app shows you the car moving toward you (Feedback). Finally, you arrive at your destination (Result).
- **Best for:** Navigation (driving to a room) or complex arm movements.

---

## 5. Parameters
Parameters are **configuration settings** for nodes. They allow you to change a node's behavior without rewriting the code.

- **Think of it as:** The "Settings" menu on your phone (changing brightness or volume).
- **Best for:** Setting the maximum speed of a robot or the IP address of a sensor.

---

## Comparison Summary

| Feature | Topics | Services | Actions |
| :--- | :--- | :--- | :--- |
| **Interaction** | Continuous Stream | Quick Q&A | Long Task |
| **Feedback** | No | No | **Yes (Continuous)** |
| **Interruptible** | No | No | **Yes (Cancelable)** |
| **Complexity** | Simple | Medium | High |
| **Example** | LIDAR scan | Set LED Color | "Drive to Kitchen" |

---

### 🔗 Official Deep-Dive Tutorials:
- [Understanding ROS 2 Nodes](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html)
- [Understanding ROS 2 Topics](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html)
- [Understanding ROS 2 Services](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.html)
- [Understanding ROS 2 Actions](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.html)
- [Understanding ROS 2 Parameters](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.html)