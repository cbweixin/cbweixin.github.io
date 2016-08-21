title: elevator-design part 1
date: 2016-08-17 11:29:09
categories:
- programming
- design
tags: system design
---

# How to design an elevator system

this is a famous interview questions. Design a elevator system would never be an easy task. How to ship people efficiently and quickly without over the load restriction, how to avoid the vacancy, how to endure the peak time. if there are multiple cars, how to allocate the floors and persons, all those questions are hard to solve. A lot of paper discussed about those problem, and a lot of patents got pulished during the investigation. some used AI, or fuzz theory. In interview, we won't have time to talk about those advanced solutions, we just need to give a feasible way, if it looks reasonable, then it is fine. 

we can start from the easiest one - only one elevator

## Elevator system with only one elevator

this is an easy case. we can design the algorithm like this way:
1. if the car is going up, the we loop all the floors which higher than the current level.if there are such floors, we would add them to our destination list.At the same time, if the lower level has requirement, we just simply don't respond to it.
2. if the car is going up, if no higher floor needs a ride, but there are lower level requirement, we can stop at the current level and change the direction and moving downward instead
3.if the car is stop for more then 30 secs, we can move the car to the first floor and waiting
4. going down could follow the same logic.

### code
the link is here : [code](https://github.com/cbweixin/leetcode-java/blob/master/src/main/java/com/weixin/ElevatorAllocationDemo.java)

#### Data Structure
```
 public int[][] controlTable;     //用二维数组controlTable与电梯口的上下键对应，其中如果值1表示button应显示绿色，值0表示button应显示灰色。
```
`controlTable` is correspoinding to buttons besides the door for each floor
```
public int[]table;               //用二维数组table与电梯内的数字键对应，其中如果值1表示button应显示桔黄色，值0表示button应显示灰色。
```

`table` stands for the buttons inside car of elevator

```java
 public class LiftThread extends Thread { 
        public int number = 1;        //电梯当前楼层。
        public int state = 0;         //电梯当前状态，0为停止，1为上升，2为下降。
        int destination;              //电梯的目的楼层。
        int i=0;
        
        public LiftThread() {
            start();
            i=octime;
            System.out.println(""+i);
        }
        
        public void run() {
            ActionListener timelistener = new TimeListener();
            timer = new Timer((1500+i), timelistener);
            timer.start();
        }
    }
```
`LiftThread` would depicts the elevator running status. `number` is the current floor number, `state` is the elevator's direction, `0-stop, 1-up, 2-down`, `destination` is the elevator's target floor. 

#### Algorithm

control logic
```java
             if (state == 1) { //向上电梯经过的向上任务完成，从任务数组中删除并重新显示上下键的颜色。
                 // notice controlTable[10-i] is buttons correspoding to the ith floor
                 // also table[10-i] is the button of ith floor inside car
                 // when stat is 1, car go up, when it arrives ith floor, if ith floor 's up control button
                 // is still on, since the car already arrived, it should be off
                if (controlTable[10 - lift.number][0] == 1) {
                    controlTable[10 - lift.number][0] = 0;
                    new openclosThread();
                    DrawControlBrick();
                }
            }
            

            if (state == 2) { //向下电梯经过的向下任务完成，从任务数组中删除并重新显示上下键的颜色。
                // when car go down, if the down control button at ith floor is still on, since the car 
                // already arrived ith floor, turn it off
                if (controlTable[10 - lift.number][1] == 1) {
                    controlTable[10 - lift.number][1] = 0;
                    new openclosThread();
                    DrawControlBrick();
                }
            }
            
            // check if the car need to continue up
            actionUp();              //判断是否继续向上 
            
            // check if the car need to continue down or not
            actionDown();            //判断是否继续向下
            
            // check car 's move direction
            state = lift.state;      //如果电梯状态不为停止，则按照运行方向运行。
            if (state == 1){
                lift.number++;
                //System.out.println("向上"+lift.number);
            }

            if (state == 2){
                lift.number--;
                //System.out.println("向下"+lift.number);
            }
            //设置相应电梯的显示器
            sign.setText("" + lift.number);
            DrawBrick();
            
            new Returnfirstfloor(state);//10秒没人按下任何键，则返回第一层
            
            }
```

algorithm to move
```java
 public class TimeListener implements ActionListener {                           
     //电梯线程的监听器。************************************
        TimeListener() {}   
        
        public void actionDown(){            
            //检查向下运行的电梯是否还需向下，即检查在此层下还有没有数字键被按下，如果没有，则置电梯状态为停止，并使该电梯内所有数字键还原。
            // if go down
            if (lift.state == 2) {
                // counter of floor which need to stop during down move
                int count = 0;
                // start from current floor , loop all the floor lower
                for (int i = lift.number; i > 0; i--)
                    // if there is a floor , the control button, no matter up or down got pressed
                    // also if inside the car, any button which is lower than current floor is pressed
                    // then we incremet the counter
                    if (table[10 - i] == 1||controlTable[10-i][1]==1||controlTable[10-i][0]==1)
                        count++;
                // if there is no such floor which need to stop during the down-move
                if (count == 0&& lift.number <= lift.destination) {
                    count=0;
                    // loop all the floor higher than current floor, if the button inside car , or control 
                    // button at higher floor is pressed, then increment counter
                    for(int j=lift.number;j<10;j++){
                        if(table[10-j]==1||controlTable[10-j][1]==1||controlTable[10-j][0]==1){
                            count++;
                        }
                    }
                    // if there exist such floor, then we need to change direction
                    if(count>0){
                        lift.state=1;
                        //System.out.println("改变方向：向上");
                        // if no need to move up, previously we already checked if the car to need to move down, so in this scenario, we could stop the car now.
                    }else{
                        lift.state = 0;
                    }
                }
            }
        }
```

move up is the same as the previous code logic,

```
 public void actionUp(){             
     //检查向上运行的电梯是否还需向上，即检查在此层上还有没有数字键被按下，如果没有，则置电梯状态为停止，并使该电梯内所有数字键还原。
            // if car is going up
            if (lift.state == 1) {
                int count = 0;
                // loop all the floor above
                for (int i = lift.number; i < 10; i++)
                    // if there is higher floor has either control button or button inside car got pressed
                    // then increment count
                    if (table[10 - i] == 1||controlTable[10-i][0]==1||controlTable[10-i][1]==1)
                        count++;                
                // if no higher floor need to car, alos the car is already at destination or above it
                if (count == 0&& lift.number >=lift.destination) {
                    count=0;
                    // loop all the floor lower than current floor , check if there is a requirement from 
                    // floor below which need the car
                    for(int j=lift.number;j>0;j--){
                        if(table[10-j]==1||controlTable[10-j][1]==1||controlTable[10-j][0]==1){
                            count++;
                        }
                    }
                    // if there is a floor below need to car
                    if(count>0){
                        // change the direction to down
                        lift.state=2;
                        //System.out.println("改变方向：向下");
                    }else{
                        // if no up or no down requirements, then  stop
                        lift.state = 0;
                    }
                }
            }
```
above discussion is about the single elevator, next part we would discuss about multiple elevators in the same buiding. 
