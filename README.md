# ROS-

### Part 1 : Manipulating Turtlesim Package in ROS Noetic

1. **Install Turtlesim Package**
   ```bash
   sudo apt install ros-noetic-turtlesim
   ```

2. **Run Turtlesim Node**
   ```bash
   roscore
   ```
   Open a new terminal:
   ```bash
   rosrun turtlesim turtlesim_node
   ```

3. **Control the Turtle**
   In a new terminal, use `rostopic` to control the turtle:
   ```bash
   rosrun turtlesim turtle_teleop_key
   ```

4. **Programming with Turtlesim**
   Create a Python script to control the turtle programmatically:
   ```python
   #!/usr/bin/env python3

   import rospy
   from geometry_msgs.msg import Twist

   def move_turtle():
       rospy.init_node('move_turtle', anonymous=True)
       pub = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=10)
       rate = rospy.Rate(1)
       
       while not rospy.is_shutdown():
           vel_msg = Twist()
           vel_msg.linear.x = 2.0
           vel_msg.angular.z = 1.0
           pub.publish(vel_msg)
           rate.sleep()

   if __name__ == '__main__':
       try:
           move_turtle()
       except rospy.ROSInterruptException:
           pass
   ```
   Save this script as `move_turtle.py`, make it executable, and run it:
   ```bash
   chmod +x move_turtle.py
   ./move_turtle.py
   ```


### Part 2 : Manipulating Turtlesim Package in ROS 2 Foxy

1. **Install Turtlesim Package**
   ```bash
   sudo apt install ros-foxy-turtlesim
   ```

2. **Run Turtlesim Node**
   ```bash
   ros2 run turtlesim turtlesim_node
   ```

3. **Control the Turtle**
   In a new terminal, use `teleop` to control the turtle:
   ```bash
   ros2 run turtlesim turtle_teleop_key
   ```

4. **Programming with Turtlesim**
   Create a Python script to control the turtle programmatically:
   ```python
   import rclpy
   from rclpy.node import Node
   from geometry_msgs.msg import Twist

   class TurtleController(Node):
       def __init__(self):
           super().__init__('turtle_controller')
           self.publisher_ = self.create_publisher(Twist, '/turtle1/cmd_vel', 10)
           timer_period = 1.0
           self.timer = self.create_timer(timer_period, self.move_turtle)

       def move_turtle(self):
           vel_msg = Twist()
           vel_msg.linear.x = 2.0
           vel_msg.angular.z = 1.0
           self.publisher_.publish(vel_msg)

   def main(args=None):
       rclpy.init(args=args)
       turtle_controller = TurtleController()
       rclpy.spin(turtle_controller)
       turtle_controller.destroy_node()
       rclpy.shutdown()

   if __name__ == '__main__':
       main()
   ```
   Save this script as `move_turtle_ros2.py`, make it executable, and run it:
   ```bash
   chmod +x move_turtle_ros2.py
   ./move_turtle_ros2.py
   ```

