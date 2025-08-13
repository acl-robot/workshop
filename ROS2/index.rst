ROS2 Beginner Workshop
======================

:Author: Kuan-Ting Lin
:Date: 2025-08-28

.. contents::
   :local:
   :depth: 2

1. Shell, Bash, and Linux Basics
--------------------------------

A **shell** is a command-line interface (CLI) that allows users to interact with the operating system's kernel by typing commands.  
It interprets these commands and translates them into actions that the system can perform.  
In Linux environments, the shell is typically text-based, although graphical shells also exist.

**bash** (Bourne Again Shell) is one of the most widely used shells in Linux and is the default in many distributions.

It provides features such as:
  
  - Command history (navigate with ↑ / ↓ arrows)
  - Tab completion for files and commands
  - Scripting capabilities with variables, loops, and conditionals

**What is a Linux distribution?**  
A **Linux distribution** (distro) is a complete operating system built around the Linux kernel, packaged with system utilities, libraries, and a package manager.  
Different distributions vary in design, default tools, and target audience.

Common distributions include:
  
  - Ubuntu
  - Debian
  - Fedora
  - Arch Linux
  - Linux Mint

**Why is Ubuntu popular for personal use?**

  - Easy installation with a graphical installer
  - Good hardware compatibility
  - Large software repositories
  - Strong community support
  - Long-Term Support (LTS) releases for stability

**Basic Linux Commands**

Below are some commonly used Linux commands with explanations:

.. code-block:: console

    cd ~/ros2_ws

.. note::

   Changes the current directory to ``~/ros2_ws``.

   The ``~`` symbol represents the current user's home directory.

   Example: ``cd /var/log`` navigates to ``/var/log``.

.. code-block:: console

    mkdir my_folder

.. note::

   Creates a new directory named ``my_folder``.

   Example: ``mkdir -p /tmp/test/data`` creates nested directories if they don't exist.

.. code-block:: console

    ls

.. note::

   Lists files and directories in the current location.

   ``ls -l`` shows detailed information (permissions, owner, size, modification date).

   ``ls -a`` includes hidden files (those starting with ``.``).

.. code-block:: console

    echo "Hello"

.. note::

   Prints the text ``Hello`` to the terminal.

   You can also redirect output: ``echo "Hello" > file.txt`` writes ``Hello`` to ``file.txt``.

.. code-block:: console

    pwd

.. note::

   Displays the full path of the current working directory.

   Useful for confirming your location in the file system.

.. code-block:: console

    touch word.txt

.. note::

   Creates an empty file named ``word.txt``.

.. code-block:: console

    rm word.txt

.. note::

   Removes the file ``word.txt``.

   Use ``rm -r folder`` to delete a folder and its contents (use with caution).

.. code-block:: console

    chmod +x filename.py

.. note::

   Adds execute (``x``) permission for all users to ``filename.py``, making it runnable (e.g., ``./filename.py``).

   - ``u`` = user (owner), ``g`` = group, ``o`` = others, ``a`` = all
   - ``r`` = read, ``w`` = write, ``x`` = execute

   You can combine these flags (like ``u+x``, ``g-w``) to control permissions for specific users.

2. ROS 2 Environment Setup
--------------------------

ROS 2 relies on combining workspaces using the shell environment.  
To use ROS 2, "source" a setup file (Linux):

.. code-block:: console

    source /opt/ros/humble/setup.bash

**Make sourcing permanent:**

.. code-block:: console

    echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

**Check your ROS environment:**

.. code-block:: console

    printenv | grep -i ROS

Output example:

.. code-block:: console

    ROS_VERSION=2
    ROS_DISTRO=humble
    ROS_PYTHON_VERSION=3

3. Using turtlesim, ros2, and rqt
---------------------------------

`turtlesim` is a simple simulator to help you learn core ROS 2 ideas.

**Install turtlesim:**

.. code-block:: console

    sudo apt update

.. code-block:: console

    sudo apt install ros-humble-turtlesim

**Check install:**

.. code-block:: console

    ros2 pkg executables turtlesim

Output:

.. code-block:: console

    turtlesim draw_square
    turtlesim mimic
    turtlesim turtle_teleop_key
    turtlesim turtlesim_node

**Run turtlesim:**

.. code-block:: console

    ros2 run turtlesim turtlesim_node

Output:

.. code-block:: console

    [INFO] [turtlesim]: Starting turtlesim with node name /turtlesim
    [INFO] [turtlesim]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]

**Open a new terminal and run:**

.. code-block:: console

    ros2 run turtlesim turtle_teleop_key

You will now see the turtle moving in the window as you use arrow keys.

.. note::

    Pressing an arrow key only moves the turtle briefly (robot safety pattern).

To see the ROS graph:

.. code-block:: console

    ros2 node list

.. code-block:: console

    ros2 topic list

.. code-block:: console

    ros2 service list

.. code-block:: console

    ros2 action list

**Install and run rqt for GUI tools:**

.. code-block:: console

    sudo apt install '~nros-humble-rqt*'

.. code-block:: console

    rqt

4. Understanding Nodes
----------------------

A **node** is a fundamental process that does some work in ROS 2.
Each node in ROS should be responsible for a single, modular purpose, e.g. controlling the wheel motors or publishing the sensor data from a laser range-finder. Each node can send and receive data from other nodes via topics, services, actions, or parameters.

.. image:: images/Nodes-TopicandService.gif

**Run two nodes (turtlesim + teleop):**
  
.. code-block:: console

    ros2 run turtlesim turtlesim_node

.. code-block:: console

    ros2 run turtlesim turtle_teleop_key

**List nodes:**
  
.. code-block:: console

    ros2 node list

Output:
  
.. code-block:: console

    /turtlesim
    /teleop_turtle

**Remap node name:**

.. code-block:: console

    ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle

**List after remap:**

.. code-block:: console

    ros2 node list

Output:

.. code-block:: console

    /my_turtle
    /turtlesim
    /teleop_turtle

**Node info:**

.. code-block:: console

    ros2 node info /my_turtle

Output (truncated):

.. code-block:: console

    /my_turtle
    Subscribers:
      /parameter_events: rcl_interfaces/msg/ParameterEvent
      /turtle1/cmd_vel: geometry_msgs/msg/Twist
    Publishers:
      /parameter_events: rcl_interfaces/msg/ParameterEvent
      /rosout: rcl_interfaces/msg/Log
      /turtle1/color_sensor: turtlesim/msg/Color
      /turtle1/pose: turtlesim/msg/Pose
    Service Servers:
      /clear: std_srvs/srv/Empty
      /kill: turtlesim/srv/Kill
      /my_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters
      /my_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
      /my_turtle/get_parameters: rcl_interfaces/srv/GetParameters
      /my_turtle/list_parameters: rcl_interfaces/srv/ListParameters
      /my_turtle/set_parameters: rcl_interfaces/srv/SetParameters
      /my_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
      /reset: std_srvs/srv/Empty
      /spawn: turtlesim/srv/Spawn
      /turtle1/set_pen: turtlesim/srv/SetPen
      /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
      /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
    Service Clients:

    Action Servers:
      /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
    Action Clients:

5. Understanding Topics
-----------------------

A **topic** is a channel for message passing between nodes.

A node may publish data to any number of topics and simultaneously have subscriptions to any number of topics.

.. image:: images/Topic-SinglePublisherandSingleSubscriber.gif

**List topics:**

.. code-block:: console

    ros2 topic list

Output:

.. code-block:: console

    /parameter_events
    /rosout
    /turtle1/cmd_vel
    /turtle1/color_sensor
    /turtle1/pose

**List topics with type:**

.. code-block:: console

    ros2 topic list -t

Output:

.. code-block:: console

    /parameter_events [rcl_interfaces/msg/ParameterEvent]
    /rosout [rcl_interfaces/msg/Log]
    /turtle1/cmd_vel [geometry_msgs/msg/Twist]
    /turtle1/color_sensor [turtlesim/msg/Color]
    /turtle1/pose [turtlesim/msg/Pose]

**Show messages on a topic (nothing shows until you move the turtle!):**

.. code-block:: console

    ros2 topic echo /turtle1/cmd_vel

Output example when moving:

.. code-block:: console

    linear:
      x: 2.0
      y: 0.0
      z: 0.0
    angular:
      x: 0.0
      y: 0.0
      z: 0.0
    ---

.. code-block:: console

    ros2 topic pub <topic_name> <msg_type> '<args>'

The ``'<args>'`` argument is the actual data you'll pass to the topic, in the structure you just discovered in the previous section.

The turtle (and commonly the real robots which it is meant to emulate) require a steady stream of commands to operate continuously.
So, to get the turtle moving, and keep it moving, you can use the following command.
It's important to note that this argument needs to be input in YAML syntax.
Input the full command like so:

**Publish a message (repeat):**

.. code-block:: console

    ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0}, angular: {z: 1.8}}"

With no command-line options, ``ros2 topic pub`` publishes the command in a steady stream at 1 Hz.

**Publish once:**

.. code-block:: console

    ros2 topic pub --once -w 2 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0}, angular: {z: 1.8}}"

``--once`` is an optional argument meaning "publish one message then exit".

``-w 2`` is an optional argument meaning "wait for two matching subscriptions".
This is needed because we have both turtlesim and the topic echo subscribed.

**Topic info:**

.. code-block:: console

    ros2 topic info /turtle1/cmd_vel

Output:

.. code-block:: console

    Type: geometry_msgs/msg/Twist
    Publisher count: 1
    Subscription count: 2

**Show message structure (type):**

.. code-block:: console

    ros2 interface show geometry_msgs/msg/Twist

Output:

.. code-block:: text

    Vector3  linear
      float64 x
      float64 y
      float64 z
    Vector3  angular
      float64 x
      float64 y
      float64 z

**View topic publish rate:**

.. code-block:: console

    ros2 topic hz /turtle1/pose

Output:

.. code-block:: console

    average rate: 59.354
      min: 0.005s max: 0.027s std dev: 0.00284s window: 58

**View topic bandwidth:**

.. code-block:: console

    ros2 topic bw /turtle1/pose

Output:

.. code-block:: console

    1.51 KB/s from 62 messages
      Message size mean: 0.02 KB min: 0.02 KB max: 0.02 KB

**Find all topics by type:**

.. code-block:: console

    ros2 topic find geometry_msgs/msg/Twist

Output:

.. code-block:: console

    /turtle1/cmd_vel

6. Understanding Services
-------------------------

A **service** is a call-response (client-server) mechanism.

.. image:: images/Service-SingleServiceClient.gif

**List all services:**

.. code-block:: console

    ros2 service list

Output:

.. code-block:: console

    /clear
    /kill
    /reset
    /spawn
    ...
    /turtle1/set_pen

**Show service type:**

.. code-block:: console

    ros2 service type /clear

Output:

.. code-block:: console

    std_srvs/srv/Empty

**List all services and types:**

.. code-block:: console

    ros2 service list -t

Output (truncated):

.. code-block:: console

    /clear [std_srvs/srv/Empty]
    /spawn [turtlesim/srv/Spawn]
    /turtle1/set_pen [turtlesim/srv/SetPen]

**Introspect service type structure:**

.. code-block:: console

    ros2 interface show turtlesim/srv/Spawn

Output:

.. code-block:: text

    float32 x
    float32 y
    float32 theta
    string name
    ---
    string name

**Call service (/clear):**

You can call a service using:

.. code-block:: console

    ros2 service call <service_name> <service_type> <arguments>

The ``<arguments>`` part is optional.
For example, you know that ``Empty`` typed services don't have any arguments:

.. code-block:: console

    ros2 service call /clear std_srvs/srv/Empty

Output (turtlesim window will clear and…)  

.. code-block:: console

    (No output unless error; window clears)

**Call service (/spawn):**

Input ``<arguments>`` in a service call from the command-line need to be in YAML syntax.

.. code-block:: console

    ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"

Output:

.. code-block:: console

    requester: making request: turtlesim.srv.Spawn_Request(x=2.0, y=2.0, theta=0.2, name='')
    response:
    turtlesim.srv.Spawn_Response(name='turtle2')

**Find all Empty services:**

.. code-block:: console

    ros2 service find std_srvs/srv/Empty

Output:

.. code-block:: console

    /clear
    /reset

7. Understanding Parameters
---------------------------

A **parameter** is a configurable value for a node.

**List all parameters:**

.. code-block:: console

    ros2 param list

Output:

.. code-block:: console

    /teleop_turtle:
      scale_angular
      scale_linear
      use_sim_time
    /turtlesim:
      background_b
      background_g
      background_r
      use_sim_time

**Get parameter value:**

.. code-block:: console

    ros2 param get /turtlesim background_g

Output (e.g.):

.. code-block:: console

    Integer value is: 86

**Set parameter:**

.. code-block:: console

    ros2 param set /turtlesim background_r 150

Output:

.. code-block:: console

    Set parameter successful

**Dump parameters:**

.. code-block:: console

    ros2 param dump /turtlesim > turtlesim.yaml

**Load parameters:**

.. code-block:: console

    ros2 param load /turtlesim turtlesim.yaml

Output:

.. code-block:: console

    Set parameter background_b successful
    Set parameter background_g successful
    Set parameter background_r successful

**Run node with parameter file:**

.. code-block:: console

    ros2 run turtlesim turtlesim_node --ros-args --params-file turtlesim.yaml

8. Understanding Actions
------------------------

Actions handle long-running, cancelable goals and provide feedback.

.. image:: images/Action-SingleActionClient.gif

**See action features in teleop_turtle node:**

Output (when you run teleop):

.. code-block:: console

    Use arrow keys to move the turtle.
    Use G|B|V|C|D|E|R|T keys to rotate to absolute orientations. 'F' to cancel a rotation.

**List actions:**

.. code-block:: console

    ros2 action list

Output:

.. code-block:: console

    /turtle1/rotate_absolute

**Show action type:**

.. code-block:: console

    ros2 action list -t

Output:

.. code-block:: console

    /turtle1/rotate_absolute [turtlesim/action/RotateAbsolute]

**Action info:**

.. code-block:: console

    ros2 action info /turtle1/rotate_absolute

Output:

.. code-block:: console

    Action: /turtle1/rotate_absolute
    Action clients: 1
      /teleop_turtle
    Action servers: 1
      /turtlesim

**Show action message structure:**

.. code-block:: console

    ros2 interface show turtlesim/action/RotateAbsolute

Output:

.. code-block:: text

    # The desired heading in radians
    float32 theta
    ---
    # The angular displacement in radians to the starting position
    float32 delta
    ---
    # The remaining rotation in radians
    float32 remaining

**Send action goal:**

Let's send an action goal from the command line with the following syntax:

.. code-block:: console

    ros2 action send_goal <action_name> <action_type> <values>

``<values>`` need to be in YAML format.

Keep an eye on the turtlesim window, and enter the following command into your terminal:

.. code-block:: console

    ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}"

Output:

.. code-block:: console

    Sending goal:
       theta: 1.57

    Goal accepted with ID: f8db8f44410849eaa93d3feb747dd444

    Result:
      delta: -1.568000316619873
    Goal finished with status: SUCCEEDED

**Send action goal with feedback:**

.. code-block:: console

    ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: -1.57}" --feedback

Output:

.. code-block:: console

    Feedback:
      remaining: -3.1268222332000732
    Feedback:
      remaining: -3.1108222007751465
    ...

    Result:
      delta: 3.1200008392333984
    Goal finished with status: SUCCEEDED

9. Recording and Playing Back Data with ros2 bag
------------------------------------------------

**ros2 bag** can record and replay data from topics.

**Create directory for bag files:**

.. code-block:: console

    mkdir bag_files

.. code-block:: console

    cd bag_files

**Record a topic:**

.. code-block:: console

    ros2 bag record /turtle1/cmd_vel

Output:

.. code-block:: console

    [INFO] [rosbag2_storage]: Opened database 'rosbag2_YYYY_MM_DD-HH_MM_SS'.
    [INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/cmd_vel'

**Record multiple topics with custom name:**

You can also record multiple topics, as well as change the name of the file ``ros2 bag`` saves to.

Run the following commands:

.. code-block:: console

    ros2 bag record -o subset /turtle1/cmd_vel /turtle1/pose

Output:

.. code-block:: console

    [INFO] [rosbag2_storage]: Opened database 'subset'.
    [INFO] [rosbag2_transport]: Listening for topics...
    [INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/cmd_vel'
    [INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/pose'
    [INFO] [rosbag2_transport]: All requested topics are subscribed. Stopping discovery...

The ``-o`` option allows you to choose a unique name for your bag file.
The following string, in this case ``subset``, is the file name.

To record more than one topic at a time, simply list each topic separated by a space.
In this case, the command output above confirms that both topics are being recorded.

**Stop recording:** (Ctrl+C)

**Bag info:**

.. code-block:: console

    ros2 bag info subset

Output (truncated):

.. code-block:: console

    Files:             subset.db3
    Bag size:          228.5 KiB
    Duration:          48.47s
    Topic information: ...
      /turtle1/cmd_vel | Type: geometry_msgs/msg/Twist | Count: 9
      /turtle1/pose    | Type: turtlesim/msg/Pose      | Count: 3004

**Play bag:**

.. code-block:: console

    ros2 bag play subset

**Check publish rate:**

.. code-block:: console

    ros2 topic hz /turtle1/pose

10. Create and Set Up a ROS2 Workspace
--------------------------------------

This section describes how to install Colcon (the ROS2 build tool), enable autocompletion, and set up your first ROS2 workspace with proper environment sourcing.

**Step-by-Step Setup**

1. **Install Colcon and Extensions**

.. code-block:: console

    sudo apt update

.. code-block:: console

    sudo apt install python3-colcon-common-extensions

.. note::
   - ``sudo apt update`` updates the package list from repositories.
   - ``sudo apt install python3-colcon-common-extensions`` installs colcon and helpful extensions.

2. **Enable Colcon Command Autocompletion**

.. code-block:: console

    cd /usr/share/colcon_argcomplete/hook

.. code-block:: console

    ls

.. note::
   - Check for the ``colcon-argcomplete.bash`` script in this directory for shell autocompletion.

Add the following to your ``~/.bashrc`` (open with e.g., gedit):

.. code-block:: bash

    source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash

Or you can use this line:

.. code-block:: bash

    echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc

Then reload your bash configuration:

.. code-block:: console

    source ~/.bashrc

.. note::
   - After this, colcon tab-completion will be available in new terminals.

3. **Create the Workspace Directory Structure**

.. code-block:: console

    mkdir -p ~/ros2_ws/src

.. code-block:: console

    cd ~/ros2_ws

.. note::
   - ``mkdir -p`` creates the directory tree for your workspace and its ``src`` folder.
   - ``cd ~/ros2_ws`` moves you into your workspace.

4. **Build the Empty Workspace**

.. code-block:: console

    colcon build

.. note::
   - Runs colcon to create initial build infrastructure even when no packages are present.
   - Generates ``build``, ``install``, and ``log`` folders.

5. **Source the Local Workspace**

.. code-block:: console

    source ~/ros2_ws/install/setup.bash

.. note::
   - This overlays your workspace’s packages in the environment for the current shell session.

6. **Make Environment Changes Persistent**

At the end of your ``~/.bashrc`` file, add these lines if not already present:

.. code-block:: bash

    source /opt/ros/humble/setup.bash
    source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash
    source ~/ros2_ws/install/setup.bash

.. note::
   - These lines ensure ROS 2 and colcon functions and your workspace are always set up in every new shell.

**You are now ready to create ROS2 packages in your own workspace!**

11. Create a ROS2 Python Package
--------------------------------

Follow these steps to create a new ROS2 Python package, optionally explore it in VS Code, and confirm that it is registered after building.

**Step-by-Step Setup**

1. **Move to the src Directory**

.. code-block:: console

    cd ~/ros2_ws/src

.. note::
   - Switch into the ``src`` folder within your workspace, where all your ROS2 source code packages should be created.

2. **Create a New Python Package**

.. code-block:: console

    ros2 pkg create my_robot_controller --build-type ament_python --dependencies rclpy

.. note::
   - ``ros2 pkg create`` generates a ROS2 package named ``my_robot_controller``.
   - The option ``--build-type ament_python`` indicates this is a Python package.
   - ``--dependencies rclpy`` adds the ROS2 Python client library as a required dependency.

3. **(Optional) Install and Launch Visual Studio Code for Exploring the Package**

.. code-block:: console

    sudo snap install code --classic

.. code-block:: console

    code .

.. note::
   - The first command installs VS Code using snap (if you don’t already have it).
   - ``code .`` opens the current folder in VS Code to allow you to visually inspect package contents and structure.

**(Recommended) Install the ROS Extension for VS Code**

After launching VS Code, it is highly recommended to install the official ROS extension for better development experience.

1. Click the Extensions icon (or press ``Ctrl+Shift+X``).
2. Search for ``ROS``.
3. Find the **ROS** extension published by Microsoft (with the blue dots as icon).
4. Click **Install**.

.. note::
   - This extension adds features like syntax highlighting, launch/URDF/parameter file support, convenient commands for building/running/testing, code navigation, and many other ROS-specific tools right inside VS Code.

4. **Go Back to the Workspace Root and Build the Package**

.. code-block:: console

    cd ~/ros2_ws

.. code-block:: console

    colcon build

.. note::
   - ``cd ~/ros2_ws`` returns to the base workspace directory.
   - ``colcon build`` compiles and installs all packages in ``src``, including your new Python package.

5. **Verify the Package Is Installed**

.. code-block:: console

    cd install

.. code-block:: console

    ls

.. note::
   - In the ``install`` directory, you should see a new folder named ``my_robot_controller``.
   - This confirms your newly created package was built and registered by the build system.

**You are now ready to write Python nodes in your custom package!**

12. Write a ROS2 Publisher with Python
--------------------------------------

This section explains how to write and run a simple ROS2 publisher node in Python to control the turtlesim robot in a circle.

**Step-by-Step Setup**

1. **Move to the Package's Source Folder and Create the Node Script**

.. code-block:: console

    cd ~/ros2_ws/src/my_robot_controller/my_robot_controller

.. code-block:: console

    touch draw_circle.py

.. code-block:: console

    chmod +x draw_circle.py

.. note::
   - Enter the Python package directory (same name as the package).
   - Use ``touch`` to create a new script ``draw_circle.py``.
   - ``chmod +x`` makes the script executable.

2. **Write the Publisher Node Code**

.. code-block:: python

    #!/usr/bin/env python3
    import rclpy
    from rclpy.node import Node
    from geometry_msgs.msg import Twist

    class DrawCircleNode(Node):
        def __init__(self):
            super().__init__("draw_circle")
            self.cmd_vel_pub = self.create_publisher(Twist, "/turtle1/cmd_vel", 10)
            self.timer = self.create_timer(0.5, self.send_velocity_command)
            self.get_logger().info("Draw circle node has been started")

        def send_velocity_command(self):
            msg = Twist()
            msg.linear.x = 2.0
            msg.angular.z = 1.0
            self.cmd_vel_pub.publish(msg)

    def main(args=None):
        rclpy.init(args=args)
        node = DrawCircleNode()
        rclpy.spin(node)
        rclpy.shutdown()
    
    if __name__ == '__main__':
        main()

.. note::
   - ``#!/usr/bin/env python3``: Shebang line to allow running script as an executable.
   - ``import rclpy``: Main ROS2 Python library.
   - ``from rclpy.node import Node``: Import ROS2 Node base class.
   - ``from geometry_msgs.msg import Twist``: Import Twist message type (for velocity commands).
   - ``class DrawCircleNode(Node)``: Define a new ROS2 node.
   - ``self.create_publisher(...)``: Create a publisher for ``/turtle1/cmd_vel`` topic.
   - ``self.create_timer(0.5, ...)``: Set up a timer to call ``send_velocity_command()`` every 0.5 seconds.
   - ``send_velocity_command()``: Create and publish a Twist message with linear and angular velocities to describe a circle.
   - The ``main`` function initializes ROS2, starts the node, and keeps it spinning.

3. **Add Dependencies to package.xml**

.. code-block:: xml

    <depend>rclpy</depend>

.. code-block:: xml

    <depend>geometry_msgs</depend>

.. code-block:: xml

    <depend>turtlesim</depend>

.. note::
   - Declare node's ROS2 and message dependencies for build and run.

4. **Update setup.py to Register the Entry Point**

.. code-block:: python

    entry_points={
        'console_scripts': [
            "draw_circle = my_robot_controller.draw_circle:main"
        ],
    },

.. note::
   - This makes your script runnable by ``ros2 run``.

5. **Build the Package and Source Your Workspace**

.. code-block:: console

    cd ~/ros2_ws

.. code-block:: console

    colcon build --symlink-install

.. code-block:: console

    source ~/.bashrc

.. note::
   - Builds your package and allows immediate source code changes to take effect (symlink mode).
   - Sourcing ``.bashrc`` ensures terminal picks up new executables.

6. **Run the Publisher Node**

.. code-block:: console

    ros2 run turtlesim turtlesim_node

.. code-block:: console

    ros2 run my_robot_controller draw_circle

.. note::
   - You should see the turtle moving in a circle in the turtlesim window, indicating your velocity publisher is working!

13. Write a ROS2 Subscriber with Python
---------------------------------------

This section shows how to write and run a simple ROS2 subscriber node in Python to listen to the turtle’s pose.

**Step-by-Step Setup**

1. **Create the Subscriber Script**

.. code-block:: console

    cd ~/ros2_ws/src/my_robot_controller/my_robot_controller

.. code-block:: console

    touch pose_subscriber.py

.. code-block:: console

    chmod +x pose_subscriber.py

.. note::
   - Enter the Python package directory.
   - Use ``touch`` to create ``pose_subscriber.py``.
   - ``chmod +x`` makes it executable.

2. **Write the Subscriber Node Code**

.. code-block:: python

    #!/usr/bin/env python3
    import rclpy
    from rclpy.node import Node
    from turtlesim.msg import Pose

    class PoseSubscriberNode(Node):
        def __init__(self):
            super().__init__("pose_subscriber")
            self.pose_subscriber = self.create_subscription(
                Pose, "/turtle1/pose", self.pose_callback, 10)
        
        def pose_callback(self, msg: Pose):
            self.get_logger().info(str(msg))

    def main(args=None):
        rclpy.init(args=args)
        node = PoseSubscriberNode()
        rclpy.spin(node)
        rclpy.shutdown()

    if __name__ == '__main__':
        main()

.. note::
   - ``#!/usr/bin/env python3``: Shebang line for executability.
   - ``from turtlesim.msg import Pose``: Import the message type for the turtle’s pose.
   - ``self.create_subscription(Pose, "/turtle1/pose", ...)``: Subscribe to the pose topic.
   - The callback logs the full pose data to the terminal.

3. **Register the Subscriber Node in setup.py**

.. code-block:: python

    entry_points={
        'console_scripts': [
            "draw_circle = my_robot_controller.draw_circle:main",
            "pose_subscriber = my_robot_controller.pose_subscriber:main",
        ],
    },

.. note::

   - Make sure each entry in the list is separated by a comma, and it is a good practice to also add a comma after the last entry. This avoids syntax errors if you add more nodes in the future.
   - With this format, both ``ros2 run my_robot_controller draw_circle`` and ``ros2 run my_robot_controller pose_subscriber`` will be properly registered and executable.

4. **Build and Source the Workspace**

.. code-block:: console

    cd ~/ros2_ws

.. code-block:: console

    colcon build --symlink-install

.. code-block:: console

    source ~/.bashrc

.. note::
   - Rebuild after any code changes.
   - Symlink install lets you test edits without reinstalling each time.
   - Sourcing the workspace updates your environment.

5. **Run the Subscription System**

Open three terminals and run:

**1st terminal:** Start turtlesim

.. code-block:: console

    ros2 run turtlesim turtlesim_node

**2nd terminal:** Start the publisher node

.. code-block:: console

    ros2 run my_robot_controller draw_circle

**3rd terminal:** Start the subscriber node

.. code-block:: console

    ros2 run my_robot_controller pose_subscriber

.. note::
   - The subscriber terminal will show live pose messages from turtlesim as the turtle moves in a circle.

Summary
-------

- You learned shell and Linux command basics.
- Set up and checked a working ROS 2 environment.
- Used turtlesim/rqt to demonstrate all core ROS 2 CLI tools and concepts.
- Key commands and feedback (output) for nodes, topics, services, parameters, actions, and bagging.

References
----------

- ROS 2 Humble official docs: https://docs.ros.org/en/humble/index.html
- ROS 2 tutorials (YouTube): https://www.youtube.com/playlist?list=PLLSegLrePWgJudpPUof4-nVFHGkB62Izy
- How to install ROS2 on Ubuntu 22.04 using VirtualBox: https://www.youtube.com/watch?v=jm8WMgwu10s&ab_channel=maiaVRobotics