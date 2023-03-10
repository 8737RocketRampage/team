# team
Work 
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot;

import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import edu.wpi.first.wpilibj.motorcontrol.MotorController;
import edu.wpi.first.wpilibj.motorcontrol.MotorControllerGroup;


//Loading the Spark max controller needed to mate Spark Max controllers work
import com.revrobotics.CANSparkMax;
import com.revrobotics.CANSparkMaxLowLevel.MotorType;





/**
 * This is a demo program showing the use of the DifferentialDrive class. Runs the motors with
 * arcade steering.
 */
public class Robot extends TimedRobot {

//Assinging Variables for each motor, then combine them into a Group
  private final CANSparkMax m_FrontLeft = new CANSparkMax(1, MotorType.kBrushed);
  private final CANSparkMax m_BackLeft = new CANSparkMax(2,MotorType.kBrushed);
  MotorController m_Left = new MotorControllerGroup(m_FrontLeft, m_BackLeft);


  private final CANSparkMax m_FrontRight = new CANSparkMax(4,MotorType.kBrushed);
  private final CANSparkMax m_BackRight = new CANSparkMax(3,MotorType.kBrushed);
  MotorController m_Right = new MotorControllerGroup(m_FrontRight, m_BackRight);

//Create a Differential Drive with both of the 2 groups we just created
  private final DifferentialDrive m_drive = new DifferentialDrive(m_Left, m_Right);

  //Setup up the Joystick to load on usb port 0
  private final Joystick m_stick = new Joystick(0);
//Setup a timer for auto
private final Timer m_timer = new Timer();



  @Override
  public void robotInit() {
    // We need to invert one side of the drivetrain so that positive voltages
    // result in both sides moving forward. Depending on how your robot's
    // gearbox is constructed, you might have to invert the left side instead.

    
    m_Right.setInverted(true);
  }

  @Override
  public void teleopPeriodic() {
    // Drive with arcade drive.
    // That means that the Y axis drives forward
    // and backward, and the X turns left and right.
   
    double robotSpeed=m_stick.getY()*.5; //Assing Axis with speed(50%) of joystick
    double robotTurn=-m_stick.getX()*.5; //Assing TUrn Axis with speed(50%) of joystick
    m_drive.arcadeDrive(robotSpeed,robotTurn); //Assing the Speed and Turn to robot Differential Drive
  }

  /** This function is run once each time the robot enters autonomous mode. */
  @Override
  public void autonomousInit() {
    m_timer.reset();
    m_timer.start();
  }

  /** This function is called periodically during autonomous. RUn all code every .2 secs */
  @Override
  public void autonomousPeriodic() {
    if (m_timer.get() < 1.0) { // Moves Forward for 2 secs
      double leftSpeed=-.5; //Slows speed by 50%
      double rightSpeed=-.5; //Slows sped byt 50%
      m_drive.tankDrive(leftSpeed, rightSpeed); // drive forwards 1/2 speed using tank Drive
    } else {
      m_drive.stopMotor(); // stop robot
     
    }

    if(m_timer.get() > 1){

     if (m_timer.get() < 2.3) {
       double leftSpeed=.5; // Left motor reverse 50% speed
       double rightSpeed=-.5; //right motor forward 50% speed
       m_drive.tankDrive(leftSpeed, rightSpeed); // Turn robot for 2 secs
     } else {
       m_drive.stopMotor(); // stop robot
     }
     }

     if(m_timer.get() > 3){

       if (m_timer.get() < 4) {
         double leftSpeed=.5; //Reverse motor 50% speed
         double rightSpeed=.5; //Reverse motor 50% speed
         m_drive.tankDrive(leftSpeed, rightSpeed); // Go backwards 2 secs at 1/2 speed
       } else {
         m_drive.stopMotor(); // stop robot
       }
       }
/** this will reset loop and run it all again
   if(m_timer.get() > 13){
     //reset timer to repeat adove commands over and over and over and over and over...ect
     m_timer.reset();
     m_timer.start();

   }
*/

  }



}
