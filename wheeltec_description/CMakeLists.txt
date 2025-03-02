cmake_minimum_required(VERSION 3.5)
project(wheeltec_description)

find_package(ament_cmake REQUIRED)

install(DIRECTORY launch/
  DESTINATION share/${PROJECT_NAME}/launch
)
install(DIRECTORY models/
  DESTINATION share/${PROJECT_NAME}/models
)
install(DIRECTORY meshes/
  DESTINATION share/${PROJECT_NAME}/meshes
)
install(DIRECTORY worlds/
  DESTINATION share/${PROJECT_NAME}/worlds
)

ament_package()