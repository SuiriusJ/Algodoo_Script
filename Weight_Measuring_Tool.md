Weight Measuring Tool (By SuiriusJ)
=======================
## Description

This script can be used to measure the weight of aircrafts, such as airships and air carriers.

Warning: Airplanes using airfoils may not be measurable.

https://github.com/SuiriusJ/Algodoo_Script/blob/master/gravitymass.txt

## How to use

    1. paste the Codes in gravitymass.txt into the console line of your object.
    
    (Only Circle, Box or Polygon: THE OBJECT MUST BE ATTACHED TO YOUR AIRCRAFT)
    
    2. Set force value of your Thruster with {scene.my.autoforce}, or add "force = scene.my.autoforce" in postStep.
    
    3. If you want to activate this script, Set '_trigger' with 1 of your object.

## Raw Code

    postStep := (e)=>{
    _trigger == 1 ? {
        scene.my.autoforce = 0;
        _TimerSet({
            _timer = 2;
            _trigger = 2;
            _TenExp = 6
        })
    } : {
        _trigger == 2 ? {
            _TimerSet({
                _timer = 2;
                _acc(1) > 0 ? {
                    _TenExp == -2 ? {
                        _trigger = 0;
                        _weight = scene.my.autoforce
                    } : {
                        scene.my.autoforce = scene.my.autoforce - (10 ^ _TenExp);
                        _TenExp = _TenExp - 1
                    }
                } : {
                    scene.my.autoforce = scene.my.autoforce + (10 ^ _TenExp)
                }
            })
        } : {
            vel(1) > 0 ? {
                scene.my.autoforce = _weight * 9 / 10
            } : {
                scene.my.autoforce = _weight * 11 / 10
            }
        }
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
                    weight = Force
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
        0:
        if vel is bigger than 0
        {
            set force with 9/10 * Force
        }
        else
        {
            set force with 11/10 * force
        }

    }

## Algorithm Description

    1. Set the external force of the aircraft to zero, and reset the acceleration of the aircraft to gravity acceleration.

    2. Continue adding 1000 to external forces in turn.

    3. If the power is working up, cancel the operation immediately before, and this time it will continue to add 100.

    4. In this way, add 10, 1, 0.1, 0.01, and calculate up to 0.01 to complete the weight measurement.

    5. In case of that _trigger is zero, if vertical velocity is positive, Force decreases, on the other hand, if vertical velocity is negative, force increases.


## Custom Variables & Functions Description

    * scene.my.autoforce: The size of the force to be delivered to the thruster

    * _oldvel: It is used to store the vel value before one tick to calculate the acceleration.
    
    * _acc: Acceleration

    * _TenExp: When calculating forces, the value added to the force

    * _timer: Delay
    
    * _SetTimer: Alternative to "Sleep();."

    * _trigger: Trigger and Phase
    
    * _weight: Size of gravity of aircraft.
