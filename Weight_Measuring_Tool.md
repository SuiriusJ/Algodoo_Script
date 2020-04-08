Weight Measuring Tool (By SuiriusJ)
=======================
## Description

이 스크립트는 비행선이나 공중모함 등, 비행 기구의 무게를 측정하는 데 사용할 수 있습니다.

경고: 에어포일을 사용하는 비행기는 측정이 불가능할 수 있습니다.

## 전체 알고리즘

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

## 의사코드

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

## 알고리즘 설명

    1. 비행 기구의 외력을 0으로 설정해, 비행기구의 가속도를 중력가속도로 초기화한다.

    2. 외력에 차례로 1000을 계속 더해간다.

    3. 만약 알짜힘이 위로 작용할 경우, 바로 전 작업을 취소하고, 이번에는 100을 계속 더해간다.

    4. 이러한 방식으로 10, 1, 0.1, 0.01을 더해가고, 0.01까지 계산하면 무게 측정을 끝낸다.


## 커스텀 변수에 대한 설명

    * scene.my.autoforce: 추진체에 전달하는 힘의 크기

    * _oldvel: 가속도를 계산하기 위해 한 틱 전의 vel값을 저장한다.
    
    * _acc: 가속도

    * _TenExp: 힘을 계산할 때의 10의 지수

    * _timer: 딜레이

    * _trigger: phase
    
    * _mass: 원래 질량을 계산하기 위해 사용되었지만, 지금은 사용하지 않음.