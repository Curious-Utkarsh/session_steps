Based on your presentation and your request, here's a complete **GitHub README.md** file for a ROS 2 project that installs turtlesim and draws a **circle** using a custom Python node. It includes everything from scratch: installing turtlesim, creating a workspace, writing the node, setting up `package.xml`, `setup.py`, and CLI commands.

---

## 🐢 ROS 2 Turtlesim Circle Drawing

This ROS 2 project demonstrates how to use the **turtlesim** simulator to draw a **circle** using a custom Python node. This project is designed for **beginners** and covers everything from installation to execution.

---

### 📦 Requirements

* Ubuntu 22.04
* [ROS 2 Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)
* `colcon` build tool
* Python 3

---

### 🛠️ 1. Install ROS 2 Humble (if not already installed)

```bash
sudo apt update && sudo apt upgrade
sudo apt install curl gnupg2 lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'

sudo apt update
sudo apt install ros-humble-desktop
```

Add ROS to your shell startup:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Install build dependencies:

```bash
sudo apt install python3-colcon-common-extensions python3-pip
```

---

### 🐢 2. Install Turtlesim

```bash
sudo apt install ros-humble-turtlesim
```

Test it:

```bash
ros2 run turtlesim turtlesim_node
```

---

### 📁 3. Create ROS 2 Workspace and Package

```bash
mkdir -p ~/turtlesim_ws/src
cd ~/turtlesim_ws/src
ros2 pkg create --build-type ament_python turtlesim_circle
```

---

### 📄 4. Add Your Python Node

Create the Python file:

```bash
mkdir -p turtlesim_circle/turtlesim_circle
touch turtlesim_circle/turtlesim_circle/circle_drawer.py
chmod +x turtlesim_circle/turtlesim_circle/circle_drawer.py
```

Paste this code in `circle_drawer.py`:

```python
#!/usr/bin/env python3

import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

class CircleDrawer(Node):
    def __init__(self):
        super().__init__('circle_drawer')
        self.publisher = self.create_publisher(Twist, '/turtle1/cmd_vel', 10)
        timer_period = 0.1
        self.timer = self.create_timer(timer_period, self.move_in_circle)

    def move_in_circle(self):
        msg = Twist()
        msg.linear.x = 2.0
        msg.angular.z = 1.0
        self.publisher.publish(msg)
        self.get_logger().info("Drawing a circle...")

def main(args=None):
    rclpy.init(args=args)
    node = CircleDrawer()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

---

### 🧾 5. Edit `package.xml`

In `turtlesim_circle/package.xml`, update:

```xml
<description>Draws a circle in turtlesim</description>
<maintainer email="you@example.com">Your Name</maintainer>
<license>Apache License 2.0</license>

<exec_depend>rclpy</exec_depend>
<exec_depend>geometry_msgs</exec_depend>
```

---

### ⚙️ 6. Edit `setup.py`

Replace contents of `turtlesim_circle/setup.py`:

```python
from setuptools import setup

package_name = 'turtlesim_circle'

setup(
    name=package_name,
    version='0.0.1',
    packages=[package_name],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='Your Name',
    maintainer_email='you@example.com',
    description='Draws a circle using turtlesim',
    license='Apache License 2.0',
    tests_require=['pytest'],
    entry_points={
        'console_scripts': [
            'draw_circle = turtlesim_circle.circle_drawer:main',
        ],
    },
)
```

---

### 📦 7. Build the Package

```bash
cd ~/turtlesim_ws
colcon build
```

Source the setup file:

```bash
source install/setup.bash
```

---

### ▶️ 8. Run the Simulation

In one terminal:

```bash
ros2 run turtlesim turtlesim_node
```

In another terminal:

```bash
ros2 run turtlesim_circle draw_circle
```

You should now see the turtle drawing a **circle** on the screen.

---

### 🧪 9. (Optional) Run with CLI (No Node)

Alternatively, to test the circular motion directly via CLI:

```bash
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0}, angular: {z: 1.0}}"
```

---

### ✅ Extra Commands

* List nodes: `ros2 node list`
* Info on node: `ros2 node info /circle_drawer`
* Topic list: `ros2 topic list`
* Echo topic: `ros2 topic echo /turtle1/cmd_vel`
* Interface: `ros2 interface show geometry_msgs/msg/Twist`

---

### 📂 Final Project Structure

```
turtlesim_ws/
├── build/
├── install/
├── log/
├── src/
│   └── turtlesim_circle/
│       ├── package.xml
│       ├── setup.py
│       └── turtlesim_circle/
│           └── circle_drawer.py
```

---

### 🐍 Add to `.bashrc` for Convenience

```bash
echo "source ~/turtlesim_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

Let me know if you'd like this exported to a `.md` file or zipped as a starter repo.

