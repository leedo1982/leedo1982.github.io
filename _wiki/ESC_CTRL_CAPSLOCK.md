---
layout  : wiki
title   : MAC caps lock 을 escape + ctrl 로 사용해보자 (for VIM user)
summary : MAC 도 해피해킹처럼 
date    : 2018-04-12 13:33:27 +0900
updated : 2018-04-15 12:06:57 +0900
tags    : hammerspoon
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 개요
해피해킹의 ctrl이 일반 키보드의 calps_lock 위치에 있어서 사용이 편리하다는 소리를 들음.
그래서 사용해보고 싶었음!!!

현재 HammerSpoon을 통해 caps_lock를 esc 로 사용중 이었기에 ctrl 기능을 추가하고자 함

[(참조:기계인간 블로그)](https://johngrib.github.io/blog/2017/05/04/input-source)


# 환경
Karabiner-Elements + hammerspoon

# logic
ctrl 은 조합키 이므로 
1. caps_lock 와 키조합이 있는 경우는 ctrl  
2. 키조합이 없는 경우는 escape 로 인식
3. caps_lock 는 modifier keys가 아니므로 karabiner를 통해 caps_lock -> rightctrl 로 변경
(rightCtrl은 macbook에서 존재하지 않는 키이므로 side-effect가 없어 선택됨)


# hammerspoon code
```


local normal_keyflag = false
local control_keyflag = false

-- 마우스 클릭 이벤트 구독 
normal_mousedownevent = hs.eventtap.new({hs.eventtap.event.types.leftMouseDown},function(event)
    local keycode = hs.keycodes.map[event:getKeyCode()]
    if(control_keyflag == true) then
        normal_keyflag = true
    elseif(control_keyflag == false) then
        normal_keyflag = false
    end

end)
normal_mousedownevent:start()

-- 키 다운 이벤트 구독 
normal_keydownevent = hs.eventtap.new({hs.eventtap.event.types.keyDown},function(event)
    local keycode = hs.keycodes.map[event:getKeyCode()]

    if(control_keyflag == true) then
        normal_keyflag = true
    elseif(control_keyflag == false) then
        normal_keyflag = false
    end
end)
normal_keydownevent:start()

control_keyevent = hs.eventtap.new({hs.eventtap.event.types.flagsChanged},function(event)
    local flags = event:getFlags()
    local keycode = hs.keycodes.map[event:getKeyCode()]

    if(flags.ctrl == true) then
        control_keyflag = true
    elseif(flags.ctrl == nil) then
        control_keyflag = false
    end

    if(flags.fn == true) then
        fnkeyflag = true
    end

    if(keycode == 'rightctrl' and flags.ctrl == nil and normal_keyflag == false) then 
        print('rightCtrl & escape !!')
        changeLanguage()
        hs.eventtap.keyStroke({}, 'escape')
        fnkeyflag = false
    end

    normal_keyflag = false

    return false
end)
control_keyevent:start()

-- 한영전환 function 
function changeLanguage()
    local inputEnglish = "com.apple.keylayout.ABC"
    local input_source = hs.keycodes.currentSourceID()

    if not (input_source == inputEnglish) then
        hs.keycodes.currentSourceID(inputEnglish)
    end
end

```
