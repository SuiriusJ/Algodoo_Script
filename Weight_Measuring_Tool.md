Weight Measuring Tool (By SuiriusJ)
=======================
## Description

This script can be used to measure the weight of aircrafts, such as airships and air carriers.

Warning: Airplanes using airfoils may not be measurable.

## Raw Code

    postStep := (e)=>{
        _trigger == 1 ? {
            scene.my.autoforce = 0;
            _TimerSet({
                _timer = 2;
                _trigger = 2;
                _TenExp = 3
            })
        } : {
            _trigger == 2 ? {
                _TimerSet({
                    _timer = 2;
                    _acc(1) > 0 ? {
                        _TenExp == -2 ? {
                            _trigger = 0
                        } : {
                            scene.my.autoforce = scene.my.autoforce - (10 ^ _TenExp);
                            _TenExp = _TenExp - 1
                        }
                    } : {
                        scene.my.autoforce = scene.my.autoforce + (10 ^ _TenExp)
                    }
                })
            } : {}
        };
        _acc = (vel - _oldvel) * sim.frequency;
        _oldvel = vel
    };


    _TimerSet := (x)=>{
        _timer <= 0 ? {x} : {
            _timer = _timer - 1
        }
    };

## Pseudo-code

    postStep := (e)=>{
        if trigger is:

        1:
        {
            Force = 0;
            Skip 2 tick
            trigger = 2;
            increasing = 1000
        }

        2:

        {
            Skip 2 tick
            if acceleration is bigger than 0 ± ERROR
            {
                if increasing is smaller than 0.01
                {
                    trigger = 0;
                    // Stop Measuring Weight
                }
                else
                {
                    increasing /= 10;
                }
            }
            else
            {
                Force += increasing
            }
        }
    }

## Algorithm description

    1. Set the external force of the aircraft to zero, and reset the acceleration of the aircraft to gravity acceleration.

    2. Continue adding 1000 to external forces in turn.

    3. If the power is working up, cancel the operation immediately before, and this time it will continue to add 100.

    4. In this way, add 10, 1, 0.1, 0.01, and calculate up to 0.01 to complete the weight measurement.


## 커스텀 변수에 대한 설명

    * scene.my.autoforce: The size of the force to be delivered to the thruster

    * _oldvel: It is used to store the vel value before one tick to calculate the acceleration.
    
    * _acc: Acceleration

    * _TenExp: When calculating forces, the value added to the force

    * _timer: Delay

    * _trigger: Phase
    
    * _mass: Originally used to calculate mass, but not used now.
