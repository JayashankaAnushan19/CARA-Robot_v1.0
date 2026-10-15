# CARA — System Overview

This is a short, non-technical overview of how CARA is organised. It describes the system at block level only. Implementation details are not public.

## The big picture

CARA is a small wheeled robot with a raised head unit. The head unit holds a camera, a small display and a keypad. Inside the robot, two controllers share the work:

- an **AI computer** (an NVIDIA Jetson board) handles vision and the higher-level features
- a **microcontroller** (an Arduino Mega board) handles real-time jobs such as driving, sensing and alerting

Dividing the work this way means the safety-related reactions stay quick and reliable while the AI does the heavier processing.

```
        ┌────────────────────────────┐
        │        Vision unit         │   camera + AI pose estimation
        │  (fall detection, person   │   live view for family
        │   tracking, phone access)  │
        └─────────────┬──────────────┘
                      │
        ┌─────────────▼──────────────┐
        │     Motion controller      │   real-time coordination
        └──┬──────────┬───────────┬──┘
           │          │           │
   ┌───────▼───┐ ┌────▼─────┐ ┌───▼────────────┐
   │ Sensing   │ │ Drive    │ │ Alert module   │  SMS over cellular
   │ obstacles │ │ wheels   │ │ (GSM/LTE)      │  no Wi-Fi needed
   │ presence  │ │          │ └────────────────┘
   └───────────┘ └──────────┘
           ┌───────────────────────────┐
           │ User interface            │  display, chime, status light,
           │                           │  keypad, greeting gesture
           └───────────────────────────┘
           ┌───────────────────────────┐
           │ Power                     │  rechargeable battery pack
           └───────────────────────────┘
```

## Blocks

| Block | What it does |
|---|---|
| **Vision unit** | Watches the room through the camera, recognises falls from body pose, keeps track of the registered user, and offers a live view to a phone browser on the home network. |
| **Motion controller** | Coordinates what the robot is doing from moment to moment and turns decisions into movement. |
| **Sensing** | A 360° ring of ultrasonic sensors detects obstacles, and motion sensors notice when someone is nearby. Obstacle safety always takes priority over movement. |
| **Drive** | Wheeled base that moves the robot around flat indoor floors. |
| **Alert module** | Sends SMS messages to family over the cellular network: fall alerts, medication follow-ups and a daily check-in. |
| **User interface** | Display messages, an audible chime and reminder, a status light, a keypad for acknowledgements, and a friendly wave when greeting someone. |
| **Power** | An on-board rechargeable battery pack powers all the blocks. |

## A typical day

1. CARA idles near the user and greets them when they come into view.
2. At set times it gives a medication reminder, and the user acknowledges it on the keypad.
3. If the vision unit sees what looks like a fall, the robot confirms it and the alert module texts family members.
4. Once a day, CARA sends family a short check-in message to say it is online.

## More information

- Project website: <https://jayashankaanushan19.github.io/CARA/>
- Development blog: <https://jayashankaanushan19.github.io/CARA/blog/>
