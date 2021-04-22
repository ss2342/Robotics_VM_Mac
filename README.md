# Robotics_VM_Mac

<h2>How to Set Up Virtual Machine on Mac For Collaborative Robotics Project</h2>

<p>You could use any virtual machien of your choice, I would suggest going with VirtualBox and VMware Fusion Player - Personal License (Free) . Since I already had VirtualBox
on my computer, I decided to give VMware a shot to see if it is any better/worse. I found them both to be equally good, but I will go with VMware since Robert and Dhruv have already posted VirtualBox instructions. 
I will post the VMware download instructions below.  </p>

<h3>Download VMWare Fusion Player - Personal Use License (Free) </h3>
<a href="https://my.vmware.com/web/vmware/evalcenter?p=fusion-player-personal">Download VMware Fusion Player - Personal Use License</a>
<p>You will just have to create a free account and you will be good to go.</p>

<h3> Download Ubuntu 20.0.4 Image </h3>
<a href="https://ubuntu.com/download/desktop">Ubuntu Desktop</a>
<p>Click the download button and it should start downloading.</p>

<h3> Create Ubuntu Enviornment on VMware Fusion</h3>
<p>Click on the "Install from disc or image" or you can drag and drop your image (will have .iso extension).
After you upload your image, click continue and set up your account details. Click Finish after you are done. </p>


<h3> Start up your Ubuntu VM</h3>
<p> Click on the play button to start your Ubuntu VM. Let the set up and installation finish and then log in with your credentials. </p>

<h3> Docker Installation </h3>
<p> Run the following commands </p>

<pre>
sudo apt-get update
sudo apt install docker.io
</pre>

<h3> Set up JdeRobotics Docker </h3>
<p> Run the following commands </p>

<p> Pull Docker Image. Make sure you have enough space on your disk! I had to go back and delete a lot of things in order to 
create space as otherwise, the download would keep failing. </p>
<pre>
sudo docker pull jderobot/robotics-academy-ros2:amazon-warehouse-multi-robot
</pre>

<p>Elevate Docker for xhost docker access</p>
<pre>
xhost +"local:docker@"
</pre>

<h3> Deploy Containers in NVIDIA drivers </h3>
<p> Run the following commands </p>

<pre>
sudo docker run -it  -e DISPLAY=$DISPLAY \
                     -e GAZEBO_MODEL_PATH=/opt/warehouse_ws/src/amazon_robot_gazebo/models:/opt/warehouse_ws/src/aws-robomaker-small-warehouse-world/models \
                     -v $XSOCK:$XSOCK \
                     -v $HOME/.Xauthority:/root/.Xauthority \
                     --privileged  --net=host --gpus all \
                        jderobot/robotics-academy-ros2:amazon-warehouse-multi-robot /bin/basht
</pre>

<p>Deploy containers to Intel drivers</p>
<pre>
sudo docker run -it \                                                         
                --env="DISPLAY" \
                --env="QT_X11_NO_MITSHM=1" \
                --env="GAZEBO_MODEL_PATH=/opt/warehouse_ws/src/amazon_robot_gazebo/models:/opt/warehouse_ws/src/aws-robomaker-small-warehouse-world/models"\
                --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
	              jderobot/robotics-academy-ros2:amazon-warehouse-multi-robot /bin/bash
</pre>

<p> <b>NOTE: For the Intel drivers, I kept getting errors with the above command so I decided to manually type it without the newline characters. 
Run the following command if you also run into that problem. It's the same thing but without the backslashes.</b>

<pre>sudo docker run -it --env="DISPLAY" --env="QT_X11_NO_MITSHM=1" --env="GAZEBO_MODEL_PATH=/opt/warehouse_ws/src/amazon_robot_gazebo/models:/opt/warehouse_ws/src/aws-robomaker-small-warehouse-world/models" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" jderobot/robotics-academy-ros2:amazon-warehouse-multi-robot /bin/bash</pre>


<p> Finally, you can run insider the container and the Rviz window should pop up.</p>
<pre>
. ./install/setup.sh
ros2 launch amazon_robot_bringup  aws_amazon_robot_multiple.py
</pre>

<p>Hope this works!</p>

<h4>Additional Resources</h4>
<a href="https://graspingtech.com/vmware-fusion-ubuntu-20.04/">Article I used as a guide to download Ubuntu VM on VMware Fusion Player</a>
<br>
<a href="http://jderobot.github.io/RoboticsAcademy/exercises/MobileRobots/multi_robot_amazon_warehouse/#contributors">Jde Multi-Robot github with setup instructions and ROS 2 Foxy download instructions</a>











