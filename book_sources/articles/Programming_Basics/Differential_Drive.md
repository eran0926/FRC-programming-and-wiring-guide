# Differential Drive
FRC的底盤主要分為三種，分別是Differential Drive、Mecanum Drive、Swerve Drive。這裡我們從簡單的Differential Drive開始。
1. 先開一個Timed Robot的Template
![{5189D05C-9F66-467D-A924-4F40D3677335}](https://hackmd.io/_uploads/SyhVJ7Ml1e.png)
2. 安裝`REVLib`Vendor Libraries (才能控制Spark Max)
![{DD26F6F0-181C-43DD-A39B-03827A55D528}](https://hackmd.io/_uploads/BJMPY7zlke.png)
3. 宣告左右兩邊的馬達
```java
private final CANSparkMax leftMotor = new CANSparkMax(0, CANSparkMax.MotorType.kBrushless);
private final CANSparkMax rightMotor = new CANSparkMax(1, CANSparkMax.MotorType.kBrushless);
```
4. 宣告Differenial Drive物件
```java
private final DifferentialDrive drive = new DifferentialDrive(leftMotor, rightMotor);
```
5. 宣告控制器(搖桿)
```java
private final XboxController driveController = new XboxController(0);
```
6. 在robotInit()中初始化馬達、設定Idle Mode、設定Inverted
```java
@Override
public void robotInit() {
    leftMotor.restoreFactoryDefaults();
    rightMotor.restoreFactoryDefaults();
    leftMotor.setIdleMode(IdleMode.kCoast);
    rightMotor.setIdleMode(IdleMode.kCoast);
    leftMotor.setInverted(false);
    rightMotor.setInverted(true);
}
```
等於
```java
@Override
public void robotInit() {
    resetMortorsFactory();
    setMortorsIdleModeBrake();
    leftMotor.setInverted(true);
    rightMotor.setInverted(false);
}
private void resetMortorsFactory(){
    leftMotor.restoreFactoryDefaults();
    rightMotor.restoreFactoryDefaults();
}

private void setMortorsIdleModeBrake(){
    leftMotor.setIdleMode(CANSparkMax.IdleMode.kCoast);
    rightMotor.setIdleMode(CANSparkMax.IdleMode.kCoast);
}
```
7. 在teleopPeriodic()中設定搖桿控制
```java
@Override
public void teleopPeriodic() {
    drive.arcadeDrive(-driveController.getLeftY(), -driveController.getRightX());
}
```