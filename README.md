# Custom End Effector
This repository contains a set of prepared packages and files which can be used to integrate a custom end-effector to a TIAGo robot. Once the apropriate modifications are made, deploy all three packages to the robot and select `custom` for the end effector type on the Web Commander.

## Packages
This repository is comprised of three different packages, which conatin a wrapper for the URDF description of the end-effector, configuration files and MoveIt configuration for the new configuration.

### custom_ee_description
This package contains a simple wrapper to include the URDF description of the custom end-effector, so the system knows its new configuration. This package must contain the following file:
* `urdf/end-effector.urdf.xacro`
  * This file must contain a xacro macro named `end_effector`, with a paramater named `parent`, which contains the name of the previous link in the kinematic chain of the arm; and an `origin` tag as its content, with the root position of the custom end-effector.

### moveit_custom_config
This package contains configuration files need for MoveIt to interact properly with the new end-effector.
* `controllers/controllers_custom.yaml`
  * This file contains a list of controllers known to MoveIt. The default `arm_controller`, `torso_controller` and `head_controller` should be left on the file, but additional controllers required by the end-effector can be added here.
*  `srdf/tiago_custom.srdf`
   * This file contains the `srdf` definition for the complete robot, defining move groups and blacklisted collisions. This file can be generated using the `moveit_setup_assistant` package, by loading the file `robots/tiago.urdf.xacro` in the `tiago_description` package and adding the `end_effector:=custom` argument.

### custom_ee_configuration
This package contains other configuration and launch files used by the TIAGo internal architecture.
* `config/approach_planner/approach_planner.yaml`
  * Contains paramters for `play_motion`, both planning groups to be used when planning the approach to the starting position via MoveIt, and the joints to be excluded (those not controlled via MoveIt).
*  `config/motions/tiago_motions.yaml`
   * Contains predefined motions for the TIAGo robot. This motions may be modified as needed, but it is strongly recommended to keep at least the original `home` motion included in the file.
