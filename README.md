# These templates assume you are going to be using them for full stack ROS2

# docker-templates
GC stands for graphics card
OS stands for operating system
Host machine instructions are given per folder. These include extra commands to run and download instructions

Why is AMD graphics card with ARM64 not on here? Because no one will ever need that

This tutorial assumes you have a file structure similar to
ProjectName/
   .devcontainer/
   build/
   install/
   log/
   src/
      all the rest of your code

AND it also assumes that when compiling with colcon, you put all your relevant folders into share/ProjectName
